---
title: SecureCRT设置端口转发原理
date: 2023-12-09 21:14:36
tags: Linux
categories: Linux
---

# SecureCRT设置端口转发原理

 <!-- more -->

## **1. 普通端口转发**

在中间服务器设置如下：

![img](/iamges/SecureCRT设置端口转发原理/1.png)

## **2. 动态端口转发（SOCKS 4 或 SOCKS 5）**

通过设置动态端口转发，中间服务器能访问的所有地址，都能通过Loacl设置的端口转发访问到，可以配合proxifer配置拦截本地127.0.0.1:12345的流量到指定的某些ip

![img](/iamges/SecureCRT设置端口转发原理/2.png)

![img](/iamges/SecureCRT设置端口转发原理/3.png)
