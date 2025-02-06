---
layout: title
title: SpringCloud-Nacos：服务注册+配置中心
date: 2024-05-01 14:32:22
tags: 框架
categories: java
---

# SpringCloud-Nacos：服务注册+配置中心

<!-- more -->

SpringCloud-Nacos：服务注册+配置中心

### 1.下载安装包

在Nacos的GitHub页面，提供有下载链接，可以下载编译好的Nacos服务端或者源代码：

GitHub主页：https://github.com/alibaba/nacos

GitHub的Release下载页：https://github.com/alibaba/nacos/releases

### 2.解压

解压后在bin目录下打开cmd，输入

```
startup.cmd -m standalone
```

启动nacos，默认用户名和密码都是nacos

nacos默认端口是8848

### 3.

在项目的父级pom加入springCloud的alibaba依赖

```xml
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.2.5.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
```

再需要使用nacos管理的微服务中加入nacos客户端依赖，并注释eruka的依赖

```xml
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>

```

再application.yml文件中加入nacos配置

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/cloud_order?useSSL=false
    username: root
    password: root
    driver-class-name: com.mysql.jdbc.Driver
  application:
    name: orderservice
  cloud:
    nacos:
      server-addr: localhost:8848

```

启动项目

![1](/iamges/1/1.png)

### 4.设置集群

在application.yml文件中设置集群名称

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/cloud_order?useSSL=false
    username: root
    password: root
    driver-class-name: com.mysql.jdbc.Driver
  application:
    name: orderservice
  cloud:
    nacos:
      server-addr: localhost:8848
      discovery:
        cluster-name: HZ #集群名称

```

当多个实例想设置多个集群时，可以在不同实例中设置不同的集群并重启对应实例

![2](/iamges/1/2.png)

orderservice使用restTemplate实现负载均衡

```java
    @Bean
    @LoadBalanced //开启负载均衡，即使一个实例也要加
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
    
        public Order queryOrderById(Long orderId) {
        // 1.查询订单
        Order order = orderMapper.findById(orderId);
//        String url = "http://localhost:8081/user/" +order.getUserId();
        //将http地址换成实例名，不用端口，使用负载均衡
        String url = "http://userservice/user/" +order.getUserId();
        User user = restTemplate.getForObject(url, User.class);
        order.setUser(user);
        // 4.返回
        return order;
    }

```

通过在orderservice配置文件中配置集群访问的策略，实现负载均衡优先使用同局域网下的集群用例

```yaml
userservice: #要做配置的微服务名称
  ribbon:
    NFLoadBalancerRuleClassName: com.alibaba.cloud.nacos.ribbon.NacosRule #负载均衡优先使用同局域网下的集群用例

```

![3](/iamges/1/3.png)

![4](/iamges/1/4.png)

![5](/iamges/1/5.png)

优先访问了同集群下的8081和8082端口的服务，8083不访问，当同集群下的服务挂了才去访问

### 5.设置开发环境

使用nacos的命名空间，将服务部署到开发环境，不会影响生成环境

![6](/iamges/1/6.png)

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/cloud_order?useSSL=false
    username: root
    password: root
    driver-class-name: com.mysql.jdbc.Driver
  application:
    name: orderservice
  cloud:
    nacos:
      server-addr: localhost:8848
      discovery:
        cluster-name: HZ #集群名称
        namespace: e915565d-ba06-4c02-823a-62bea0e82947 #开发环境

```

namespace对应的是生成的命名空间id， orderservice启动后就会把服务部署到dev环境，orderservice的服务便不会访问到public空间下的服务了（隔离）

### 6. 配置配置中心

（配置中心需要先把服务注册上一次，再使用bootstrap.yml文件配置）

在nacos中新建配置文件，DataID格式为 服务名-环境.yaml

![7](/iamges/1/7.png)

因为实现配置中心必须spring需要先获取nacos配置才再去获取application.yml，获取nacos配置需要在读取application.yml之前完成，因此将nacos的配置信息放在bootstrap.yml中（读取优先级比application.yml更高）

bootstarp.yml
```yaml
spring:
  application:
    name: userservice #服务名
  profiles:
    active: dev #环境
  cloud:
    nacos:
      server-addr: localhost:8848 #ip地址
      config:
        file-extension: yaml #配置文件后缀

```

并把application中的nacos信息去掉

测试：

![8](/iamges/1/8.png)

结果：

![9](/iamges/1/9.png)

#### 实现配置的热更新：

**方式一：**

在@Value注入的变量所在的类上添加注解@RefreshScope

![10](/iamges/1/10.png)

![11](/iamges/1/11.png)

![12](/iamges/1/12.png)

实现日期格式热更新

**方式二：**

使用@ConfigurationProperties注解 写一个配置类或者相关配置

```java
@Data
@Component
@ConfigurationProperties(prefix = "pattern")
public class PatternProperties {
    //变量名就是配置名
    private String dateoformat;
}
```

获取配置信息

![13](/iamges/1/13.png)

### 7. 配置文件多环境共享

nacos在启动时会加载配置中心的两种类型的配置文件

1. 服务名+环境.文件后缀
2. 服务名.文件后缀

因此可以配置userservice.yaml作为userservice的多环境共享配置文件

![14](/iamges/1/14.png)

在PatternProperties类中增加字段envShareValue

![15](/iamges/1/15.png)

编写测试接口

![16](/iamges/1/16.png)
将8082的userservice服务设为test环境作为对比

![17](/iamges/1/17.png)

结果：dev环境下可以访问到全部，test只能访问到共享的配置文件

![18](/iamges/1/18.png)

![19](/iamges/1/19.png)
