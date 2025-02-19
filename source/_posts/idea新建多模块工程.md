---
title: idea新建多模块工程
date: 2022-06-01 14:32:22
tags: 框架
categories: java
---

# idea新建多模块工程

<!-- more -->

idea新建多模块工程

### 创建继承关系模块(两个模块具有父子关系)

#### 创建父子模块

##### 第一步：创建父模块

依次点击 File -> New -> Project ->Maven ->Next

![image-20250209211600108](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20250209211600108.png)

![image-20250209211634556](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20250209211634556.png)

创建后目录结构如下：

![image-20250209211718824](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20250209211718824.png)

##### 第二步：修改父模块pom

在修改pom文件之前，先删掉父模块中的src目录。

![image-20250209211754091](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20250209211754091.png)

然后在修改pom文件，如果只是简单能用，其实只要修改如下两个配置即可：

1、添加标签并将其改为pom。

2、添加标签配置对所有模块依赖的管理。

修改后完整pom如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>netty-im</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <!-- 当前工程作为父工程，它要去管理子工程，所以打包方式必须是 pom -->
    <packaging>pom</packaging>

    <!-- 子模块 -->
    <modules>
        <module>redis-learn</module>
        <module>pro-common</module>
    </modules>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <!-- 二选一，使用dependencyManagement标签配置对所有模块依赖的管理，单独选择 -->
    <dependencyManagement>
        <dependencies>
            <!-- spring-boot 依赖 -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>2.3.3.RELEASE</version>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
                <version>2.3.3.RELEASE</version>
                <scope>compile</scope>
            </dependency>
            <!-- spring redis 依赖 -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-data-redis</artifactId>
                <version>2.3.3.RELEASE</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
    
      <!-- 二选一，也可以直接引入 Spring Boot 的依赖管理，供所有子模块统一使用 -->
      <dependencyManagement>
        <dependencies>
          <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>${spring-boot.version}</version>
            <type>pom</type>
            <scope>import</scope>
          </dependency>
        </dependencies>
      </dependencyManagement>

</project>

```

#### 创建子模块

##### 第一步：创建子模块

选中父工程 pro-learn -> New -> Module -> Spring Initializr -> Module SDK(选择自己的jdk版本) -> Next

![image-20250209214045825](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20250209214045825.png)

按自己需求填写如下信息，点击Next进入下一步。

![image-20250209214214215](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20250209214214215.png)

这里可以什么都不选，点击Next进入下一步，然后，按自己需求填写信息，最后点击create完成子模块的创建。

![image-20250209214254923](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20250209214254923.png)

创建子模块后的目录结构如下：

![image-20250209221227155](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20250209221227155.png)

##### 第二步：父模块中添加modules管理子模块

在父pom中添加如下配置：

```xml
    <!--子模块-->
    <modules>
        <module>im-common</module>
    </modules>
```

##### 第三步：配置子模块pom

将redis-learn模块pom文件中parent中的坐标(groupId、[artifactId](https://so.csdn.net/so/search?q=artifactId&spm=1001.2101.3001.7020)、version)改为父模块的坐标，及添加依赖，具体看以下pom文件中的注释：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!-- 使用parent标签指定当前工程的父工程 -->
    <parent>
        <!-- 父工程的坐标 -->
        <groupId>com.im</groupId>
        <artifactId>netty-im-parent</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>

    <!-- 子工程的坐标 -->
    <!-- 如果子工程坐标中的groupId和version与父工程一致，可以省略 -->
    <modelVersion>4.0.0</modelVersion>
    <artifactId>im-common</artifactId>
    <packaging>jar</packaging>

    <!-- 子工程引用父工程中的依赖信息时，可以把版本号去掉。	-->
    <!-- 把版本号去掉就表示子工程中这个依赖的版本由父工程决定。 -->
    <!-- 具体来说是由父工程的dependencyManagement来决定。 -->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```



### 创建依赖关系模块(一个模块引用另一个模块)

在我们开发中，是不是经常把一些公用的方法，比如你的util工具类，常量类等放到一个单独的包中，比如放到common包中，如果分多模块，就可以把这些公共方法单独放到一个模块中。

#### 创建子模块

##### 第一步：创建子模块

![image-20250209221603085](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20250209221603085.png)

##### 第二步：在im-platform引用im-common

修改im-platform模块中的pom文件，通过pro-common的坐标来引用im-common模块，在im-platform的pom中添加如下配置：

```xml
		<dependency>
			<groupId>com.im</groupId>
			<artifactId>im-common</artifactId>
			<version>0.0.1-SNAPSHOT</version>
		</dependency>

```

至此，一个基本的maven管理的多模块项目就搭建完毕了。
