---
title: 工程输出中文乱码，全局各种配置UTF-8后依旧没作用
date: 2023-02-01 20:14:36
tags: java
categories: java
---

# 工程输出中文乱码，全局各种配置UTF-8后依旧没作用

<!-- more -->

**旧开发的坑**

**首先检查工程之前的编码，以前的开发没注意创建一些文件时会直接默认成GBK编码，打开项目，idea编译器会提示为GBK编码，中文显示乱码，一开始就设置项目为GBK才能看到中文显示，而现在开发大部分默认设置成UTF-8，**

**所以新建文件都是UTF-8，编译走了UTF-8后执行就会显示成中文乱码，暴力解决思路，把涉及到的文件删除新建，或者整个工程重新新建，一开始就默认成了UTF-8就能解决一切**

![img](/iamges/工程输出中文乱码，全局各种配置UTF-8后依旧没作用/1.png)

**因为文件编码与全局编码设置不一样，重新建一个文件覆盖这个同名文件即可，可能是之前开发建文件时的全局编码设置为gbk导致，然后现在开发设置的全局编码普遍为utf-8，所以编译执行后中文就会乱码**

#############################################

**常规解决**

**解决方案一、****项目设置pom文件编译的编码格式为utf-8**

在maven项目的pom.xml文件设置编译插件及项目编码<encoding>UTF-8</encoding>，具体如下图所示

```
<build>
        <!-- 插件 -->
        <plugins>
            <!-- 编译插件 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <compilerVersion>${java.version}</compilerVersion>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                    <encoding>UTF-8</encoding>
                    <!-- prevents endPosTable exception for maven compile -->
                    <useIncrementalCompilation>false</useIncrementalCompilation>
                </configuration>
            </plugin>
        </plugins>
</build>
```

**解决方案二、idea设置File Encodings为utf-8**

idea打开配置，搜索encode，配置如下图所示：

![img](/iamges/工程输出中文乱码，全局各种配置UTF-8后依旧没作用/2.png)

**解决方案三、tomcat启动配置设置VM编码参数**

编辑tomcat启动配置，添加VM参数：-Dfile.encoding=UTF-8

![img](/iamges/工程输出中文乱码，全局各种配置UTF-8后依旧没作用/3.png)

