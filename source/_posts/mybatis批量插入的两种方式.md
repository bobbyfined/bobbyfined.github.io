---
title: mybatis批量插入的两种方式
date: 2023-08-01 14:19:36
tags: mybatis
categories: java
---

# mybatis批量插入的两种方式

<!-- more -->

# 一. mybatis里的foreach标签

foreach的主要用在构建in条件中，它可以在SQL语句中进行迭代一个集合。foreach元素的属性主要有 item，index，collection，open，separator，close。item表示集合中每一个元素进行迭代时的别名，index指 定一个名字，用于表示在迭代过程中，每次迭代到的位置，open表示该语句以什么开始，separator表示在每次进行迭代之间以什么符号作为分隔 符，close表示以什么结束，在使用foreach的时候最关键的也是最容易出错的就是collection属性，该属性是必须指定的，但是在不同情况 下，该属性的值是不一样的，主要有一下3种情况：

如果传入的是单参数且参数类型是一个List的时候，collection属性值为list

如果传入的是单参数且参数类型是一个array数组的时候，collection的属性值为array

如果传入的参数是多个的时候，我们就需要把它们封装成一个Map了

具体用法如下:

```mysql
 <insert id="insertBatch" parameterType="List">              

INSERT INTO TStudent(name,age)

<foreach collection="list" item="item" index="index" open="("close=")"separator="union all">

SELECT #{item.name} as a, #{item.age} as b FROM DUAL

</foreach>

</insert>
```



# 二、使用mybatis ExecutorType.BATCH

mybatis内置的ExecutorType有三种模式，默认为simple，该模式下它为每个语句的执行创建一个新的预处理语句，单条提交sql；而batch模式重复使用已经预处理的语句，并且批量执行所有更新语句，显然batch性能将更优； 但batch模式也有自己的问题，比如在Insert操作时，在事务没有提交之前，是没有办法获取到自增的id，这在某型情形下是不符合业务要求的

```java
//获取sqlsession

//从spring注入原有的sqlSessionTemplate

@Autowired

private SqlSessionTemplate sqlSessionTemplate;

// 新获取一个模式为BATCH，自动提交为false的session

// 如果自动提交设置为true,将无法控制提交的条数，改为最后统一提交，可能导致内存溢出

SqlSession session = sqlSessionTemplate.getSqlSessionFactory().openSession(ExecutorType.BATCH,false);

//通过新的session获取mapper

fooMapper = session.getMapper(FooMapper.class);

int size = 10000;

try{

for(int i = 0; i < size; i++) {

Foo foo = new Foo();

foo.setName(String.valueOf(System.currentTimeMillis()));

fooMapper.insert(foo);

if(i % 1000 == 0 || i == size - 1) {

//手动每1000个一提交，提交后无法回滚 

session.commit();

//清理缓存，防止溢出

session.clearCache();

}

}

} catch (Exception e) {

//没有提交的数据可以回滚

session.rollback();

} finally{

session.close();

}

spring+mybatis


```

