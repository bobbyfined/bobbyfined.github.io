---
title: MYSQL 解锁与锁表和批解锁
date: 2023-08-01 14:14:36
tags: mysql
categories: 数据库
---

# MYSQL 解锁与锁表和批解锁

 <!-- more -->

重启是可以解决表被锁的问题的，但针对线上业务很显然不太具有可行性。

下面来看看不用跑路的解决方案：

## 第一步：查看表使用

遇到数据库阻塞问题，首先要查询一下表是否在使用。

复制

```
show open tables where in_use > 0 ;1.
```

如果查询结果为空，那么说明表没在使用，说明不是锁表的问题。

复制

```
ysql>  show open tables where in_use > 0 ;

Empty set (0.00 sec)1.2.
```

如果查询结果不为空，比如出现如下结果：

复制

```
mysql>  show open tables where in_use > 0 ;

+----------+-------+--------+-------------+

| Database | Table | In_use | Name_locked |

+----------+-------+--------+-------------+

| test     | t     |      1 |           0 |

+----------+-------+--------+-------------+

1 row in set (0.00 sec)1.2.3.4.5.6.7.
```

则说明表(test)正在被使用，此时需要进一步排查。

## 第二步：查看进程

查看数据库当前的进程，看看是否有慢SQL或被阻塞的线程。

执行命令：

复制

```
show processlist;1.
```

该命令只显示当前用户正在运行的线程，当然，如果是root用户是能看到所有的。

在上述实践中，阿里云控制台之所以能够查看到所有的线程，猜测应该使用的就是root用户，而笔者去kill的时候，无法kill掉，是因为登录的用户非root的数据库账号，无法操作另外一个用户的线程。

## 第三步：查看当前运行的所有事务

如果情况紧急，此步骤可以跳过，主要用来查看核对：

复制

```
SELECT * FROM information_schema.INNODB_TRX;1.
```

## 第四步：查看当前出现的锁

如果情况紧急，此步骤可以跳过，主要用来查看核对：

复制

```
SELECT * FROM information_schema.INNODB_LOCKs;1.
```

## 第五步：查询锁等待的对应关系

复制

```
SELECT * FROM information_schema.INNODB_LOCK_waits;1.
```

看事务表INNODB_TRX中是否有正在锁定的事务线程，看看ID是否在show processlist的sleep线程中。如果在，说明这个sleep的线程事务一直没有commit或者rollback，而是卡住了，需要手动kill掉。

搜索的结果中，如果在事务表发现了很多任务，最好都kill掉。

## 第六步：kill掉事务

执行kill命令：

复制

```
kill 1011;1.
```

对应的线程都执行完kill命令之后，后续事务便可正常处理。

针对紧急情况，通常也会直接操作第一、第二、第六步。

## MySQL的锁

这里再补充一些MySQL锁相关的知识点：数据库锁设计的初衷是处理并发问题，作为多用户共享的资源，当出现并发访问的时候，数据库需要合理地控制资源的访问规则，而锁就是用来实现这些访问规则的重要数据结构。

根据加锁的范围，MySQL里面的锁大致可以分成全局锁、表级锁和行锁三类。MySQL中表级别的锁有两种：一种是表锁，一种是元数据锁(metadata lock，MDL)。

表锁是在Server层实现的，ALTER TABLE之类的语句会使用表锁，忽略存储引擎的锁机制。表锁通过lock tables… read/write来实现，而对于InnoDB来说，一般会采用行级锁。毕竟锁住整张表影响范围太大了。

另外一个表级锁是MDL(metadata lock)，用于并发情况下维护数据的一致性，保证读写的正确性，不需要显式的使用，在访问一张表时会被自动加上。

## MySQL锁表场景

常见的一种锁表场景就是有事务操作处于：Waiting for table metadata lock状态。

Waiting for table metadata lock

MySQL在进行alter table等DDL操作时，有时会出现Waiting for table metadata lock的等待场景。

一旦alter table TableA的操作停滞在Waiting for table metadata lock状态，后续对该表的任何操作(包括读)都无法进行，因为它们也会在Opening tables的阶段进入到Waiting for table metadata lock的锁等待队列。如果核心表出现了锁等待队列，就会造成灾难性的后果。

### 场景一：长事务运行，阻塞DDL，继而阻塞所有同表的后续操作。

通过show processlist可以看到表上有正在进行的操作(包括读)，此时alter table语句无法获取到metadata 独占锁，会进行等待。

### 场景二：为提交事务，阻塞DDL，继而阻塞所有同表的后续操作。

通过show processlist看不到表上有任何操作，但实际上存在有未提交的事务，可以在

information_schema.innodb_trx中查看到。在事务没有完成之前，表上的锁不会释放，alter table同样获取不到metadata的独占锁。

处理方法：通过 select * frominformation_schema.innodb_trx\G, 找到未提交事物的sid，然后kill掉，让其回滚。

### 场景三：显式事务失败操作获得锁，未释放

通过show processlist看不到表上有任何操作，在

information_schema.innodb_trx中也没有任何进行中的事务。很可能是因为在一个显式的事务中，对表进行了一个失败的操作(比如查询了一个不存在的字段)，这时事务没有开始，但是失败语句获取到的锁依然有效，没有释放。从performance_schema.events_statements_current表中可以查到失败的语句。

处理方法：通过performance_schema.events_statements_current找到其sid，kill 掉该session，也可以kill掉DDL所在的session。

总之，alter table的语句是很危险的(核心是未提交事务或者长事务导致的)，在操作之前要确认对要操作的表没有任何进行中的操作、没有未提交事务、也没有显式事务中的报错语句。

如果有alter table的维护任务，在无人监管的时候运行，最好通过lock_wait_timeout设置好超时时间，避免长时间的metedata锁等待。

## 小结

关于MySQL的锁表其实还有很多其他场景，我们在实践的过程中尽量避免锁表情况的发生，当然这需要一定经验的支撑。但更重要的是，如果发现锁表我们要能够快速的响应，快速的解决问题，避免影响正常业务，避免情况进一步恶化。所以，本文中的解决思路大家一定要收藏或记忆一下，做到有备无患，避免突然状况下抓瞎。

