---
title: java spi
date: 2023-02-01 21:14:36
tags: java
categories: java
---

#  java spi

<!-- more -->

**SPI组成**

spi机制由三个组件形成，分别是service(公开的接口或者抽象类)， service Provider（接口的实现类），ServiceLoader（核心组件，负责在运行时加载并发现实现类）

**运行流程**

![img](/iamges/java spi/1.png)

application调用serviceLoader加载实现类， 最终application拿到的是service接口，不关心具体的实现

java spi在JDBC中的应用：

出现前：

```
Class.forName("驱动名");
```

出现后，直接添加依赖即可！

![img](/iamges/java spi/2.png)

**java spi三大规范要素**

一、

路径：配置文件的文件路径必须在JAR包中的META-INF/services目录下；

名称：service接口的全限定名

内容：service接口实现类的全限定名，每个实现类都需要单独占据一行

二、

service接口的实现类必须具备无参的默认构造方法，因为之后通过反射技术实例化它时，是不带参数的

三、

保证能加载到配置文件和接口实现类

方式1：将接口实现类的jar包放在classpath中

方式2：将jar包安装在jre的扩展目录中

方式3：自定义一个classLoader

## **作用**

提供一种组件发现和注册的方式，可以用于实现各种插件，或者灵活替换框架所使用的组件

## **优点**

基于面向接口编程，优雅地实现模块之间的解耦

## **设计思想**

面向接口 + 配置文件 + 反射技术

## **应用场景**

JDBC实现（导入maven依赖）, 切换实现api项目

例子

SPIdemo

定义一个接口模块

![img](/iamges/java spi/3.png)

引入接口模块依赖并创建两个实现类模块，并在classpath路径下创建META-INF services目录 创建对应配置文件

![img](/iamges/java spi/4.png)

![img](/iamges/java spi/5.png)

![img](/iamges/java spi/6.png)

![img](/iamges/java spi/7.png)

![img](/iamges/java spi/8.png)

创建application调用其中的实现类(通过类加载器加载接口，只关注接口)

引入对应实现类的依赖

![img](/iamges/java spi/9.png)

![img](/iamges/java spi/10.png)

## **Java SPI与springboot的自动配置**

spring启动类默认扫描的是所在包下的类

![img](/iamges/java spi/11.png)

