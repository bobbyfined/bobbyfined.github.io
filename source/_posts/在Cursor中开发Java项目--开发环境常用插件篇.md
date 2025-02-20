---
title: 在Cursor中开发Java项目 -- 开发环境常用插件
date: 2025-02-08 20:32:22
tags: cursor
categories: cursor

---

# 在Cursor中开发Java项目 -- 开发环境常用插件

<!-- more -->

在 Cursor 下开发 Java项目主要依赖 vs code 插件，以下主要介绍一些在Java项目开发下常用的插件：

- Extension Pack for Java：Java环境插件包(语言功能/lombok/debug/test/项目管理/maven/gradle/refactor)

- maven-dependency-explorer：maven 项目依赖分析

- Gitlens：git功能增强

- IntelliJ IDEA Keybindings：IDEA 快捷键

- SQLTools / SQL Notebook：数据库连接查询工具插件

- Spring Boot Extension Pack：Spring boot 工程支撑插件包（springboot项目脚手架/dashboard等）


# 一、JAVA语言支持插件

建议安装 Extension Pack for Java 插件大礼包，包含以下插件：

- Language support for java by red hat：

- Debuger for java, 

- Test Runner for Java, 

- Maven for java, 

- Gradle for Java, 

- Project Manager for Java,

- IntelliCode

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/1.png)

## Java配置

安装之后 (在settings.json中增加) 相关配置参考：

```
 "java.jdt.ls.java.home": "/path/to/jdk21_install_path/",
    "java.completion.enabled": true,
    "java.configuration.runtimes": [
        {
            "name": "JavaSE-1.8",
            "path": "/path/to/jdk_install_path",
            "default": true
        },
        {
            "name": "JavaSE-1.7",
```



# 二、项目相关支持

基于 Project Manager for Java 插件，提供项目创建、依赖管理、打包、结构预览等功能

（当然现在比较常用的还是使用maven/gradle管理项目，如果需要创建普通Java项目，那么还是由该插件支持）

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/2.png)

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/3.png)

# 三、lombok支持

常用的 lombok 直接集成进 Language support for Java语言插件里面去了，所以很容易使用

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/4.png)

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/5.png)

# 四、Debug 开发调试模式

由 Debuger for java 插件支持

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/6.png)

相关的断点条件也支持

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/8.png)

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/9.png)

# 五、maven编译插件

Maven for java 插件支持常见 clean, compile, test, package, install, deploy 等常见命令，跟 IntelliJ IDEA 类似

## 1. maven配置

在用户配置文件中(settings.json)加入配置：

```
    "java.configuration.maven.globalSettings": "/path/to/maven_install_path/conf/settings.xml",
    "maven.executable.path": "/path/to/maven_install_path/bin/mvn",
```



## 2. JAVA_HOME 设置

由于Java语言插件服务本身需要JDK17之后的版本，Maven for java 插件是跟语言插件相关的，所以，建议本机 JAVA_HOME 设置成 JDK17以后的版本，可以避免很多问题

## 3. 依赖分析

对于 IDEA 下常用的依赖分析，可以直接导出 effective pom 来搜索，包含了完整的所有依赖包，下面还有一个插件maven-dependency-explorer 功能更强大

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/10.png)

## 4. 安全漏洞报告分析

插件支持分析依赖中包含的安全漏洞报告

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/11.png)

## 5. 新建 maven 项目流程

这里直接引用之前的另一个文档中的在 cursor 新建 maven 项目流程：

使用 ctrl + shift + p 打开命令框，输入“java: create java project”，回车，

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/12.png)

选择 maven，回车

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/13.png)

选择项目原型： maven-archetype-quickstart （常用），回车

选最新版本，回车

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/14.png)

然后输入创建 maven 项目的 group id, artifact id, 项目保存路径等信息，回车，maven 插件就会开始在命令行中执行 maven 项目创建的命令，

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/15.png)

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/16.png)

注意此时 maven 插件会提示确认以上信息，可以直接回车确认

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/17.png)

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/18.png)

然后弹框提示是否在新窗口打开新创建的maven项目

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/19.png)

打开项目之后如下图，默认生成了一个 App.java 类，

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/20.png)

此时点击 Run | Debug ，可以测试一下开发环境是否可以编译执行，正常会在终端命令行得到以下结果（vs-code 环境比起 Idea 环境展示了更多的真实的执行信息，没有把他们隐藏在UI背后）

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/21.png)

## 6. 打包

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/22.png)



![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/23.png)

# 六、maven 依赖分析

使用 maven-dependency-explorer 插件

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/24.png)

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/25.png)

# 七、代码托管：gitlab

## 1. 新建gitlab远程仓库

在 gitlab/github仓库上创建远程仓库，不截图展示了

## 2. 本地项目初始化 git 目录

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps1.jpg)

## 3. 安装 GitLens 插件

GitLens 是vscode下一款对git操作支持较强的插件，本身收费，可以进行使用其完整功能，安装完成之后，左边“源码管理tab”下会多出几个栏

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps2.jpg)

此处贴上一篇对于gitlens使用讲解很详细的，对于git的一些常用操作也有很好的补充讲解，以下是一些演示：

### a) add & commit

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps3.jpg)![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps4.jpg)

### b) push 到远程仓库

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps5.jpg)![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps6.jpg)

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps7.jpg) 

### c) 从远程仓库拉取更新

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps8.jpg)

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps9.jpg) 

### d) 遇到冲突处理

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps10.jpg)

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps11.jpg) 

在起冲突的情况下，gitlens不允许直接pull更新或者push，需要先解决冲突或者想办法回退本地更改(可以将本地commit回退，然后创建stash暂存起来)之后pull，再push

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/26.png)

# 八、快捷键：IntelliJ IDEA Keybindings

支持使用 IntelliJ IDEA 快捷键

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/27.png)

# 九、在Cursor下查询数据库

## 1. 插件：SQLTools

支持连接多种数据源，安装之后在左侧panel中会出现一个连接预览面板

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/28.png)

点击 add new connection 右边选择 driver，一开始默认没有任何 driver，点击 search vs code marketplace，左边自动搜索可用 driver

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/29.png)

安装 mysql、clickhouse 等常用 driver

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/30.png)

新建一个本地mysql连接测试一下

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/31.png)

界面简洁清晰

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/32.png)

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/33.png)

## 2. 插件：SQL Notebook

支持类似 python jupyter notebook 风格的操作界面，支持 mysql等多种数据库

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/34.png)

新建一个测试sql文件，右键 open with ... SQL Notebook

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/35.png)

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/36.png)

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/37.png)



## 3. SQLTools、SQL Notebook 两个插件也可以一起使用，

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/38.png)

以上在 SQL Notebook 下编辑保存之后得到的就是一个 sql 文本文件，可多次编辑保存，提交 git 方便版本控制等等

![img](/iamges/在Cursor中开发Java项目--开发环境常用插件篇/39.png)

# 十、Java开发重构

Language support for java by red hat 插件支持重构的常见各种操作，选中需要重构的代码，右键 Refactor (ctrl + T)，会根据代码情况出现相关提示框进行相关操作

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps12.jpg)

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps13.jpg) 

# 十一、开发单元测试

由 Test Runner for java 插件支持，安装之后左侧 panel 会出现 Test Explorer 面板，列出项目中的单元测试列表，方便执行操作

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps14.jpg)

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps15.jpg) 

# 十二、Spring应用开发

## spring 项目脚手架：Spring Initializr Java Support

ctrl + shift + p，打开命令框，输入 create java project，然后选择 :

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps16.jpg)

spring boot 工程 > maven project > springboot 版本号 > java 开发语言 > groupId > artifactId > 打包类型(jar) > JDK 版本(看起来现在只支持17及以后的版本了) > 预选依赖(web, mysql, jdbc, redis ...) > 项目存放路径 > 然后新窗口打开项目

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps17.jpg)![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps18.jpg)

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps19.jpg)![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps20.jpg) 

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps21.jpg) 

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps22.jpg) 

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps23.jpg)![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps24.jpg) 

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps25.jpg) 

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps26.jpg) 

## springboot 应用程序看板：springboot dashboard

启动应用程序之后在左边panel下打开 springboot dashboard，会显示应用程序中对应的bean, api接口, 配置信息等

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps27.jpg)

## springboot 项目开发工具：spring boot tools

安装插件：Spring Boot Tools

安装之后，对于springboot项目常用的注解、配置等会有自动提示，

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps28.jpg) 

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps29.jpg) 

ctrl + shift + o 快速查看都有哪些接口可以访问，选中可以快速跳转访问测试

![img](file:///C:\Users\JIA\AppData\Local\Temp\ksohtml8944\wps30.jpg)

注：ctrl+shift+o快捷键本来绑定的操作是文件/变量/符号等跳转，spring boot tools 插件给这个操作添加了一些：url、rest接口路径mapping 绑定的跳转

（快捷键有可能跟IntelliJ IDEA keybindings 插件定义的一些快捷键冲突，稍为调整一下即可）

