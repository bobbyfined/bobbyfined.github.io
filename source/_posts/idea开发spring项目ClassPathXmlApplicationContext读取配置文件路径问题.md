---
title: idea开发spring项目ClassPathXmlApplicationContext读取配置文件路径问题
date: 2024-06-01 21:41:50
tags: 框架
categories: java
---

# idea开发spring项目ClassPathXmlApplicationContext读取配置文件路径问题

<!-- more -->

将applicationContext.xml文件放在src目录下，编译后从生成的target可以看到classes目录（存放.class文件）并没有applicationContext.xml，因为编译只会对.java文件编译

![1](/iamges/2/1.png)

而application.[xml](https://so.csdn.net/so/search?q=xml&spm=1001.2101.3001.7020)文件放在resource目录下，IDEA就会将xml文件复制到classess文件夹下，就能正常访问了。

![2](/iamges/2/2.png)

若想要配置不同的applicationContext.xml读取路径，则可以创建相应的包目录，指定对应的包路径就可以访问

![3](/iamges/2/3.png)

![4](/iamges/2/4.png)
