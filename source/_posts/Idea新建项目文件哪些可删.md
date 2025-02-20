---
title: Idea新建项目文件哪些可删
date: 2022-01-01 14:32:22
tags: 框架
categories: java
---

# Idea新建项目文件哪些可删

<!-- more -->

## Idea新建项目文件解释

![img](/iamges/Idea新建项目文件哪些可删/1.png)

- .gitignore 用git做版本控制时 用这个文件控制那些文件或文件夹 不被提交（不用git的话可删除 没影响）

- HELP.md md是一种文档格式 这个就是你项目的帮助文档（可删除 没影响）

- mvnw linux上处理mevan版本兼容问题的脚本（可删除 没影响）

- mvnw.cmd windows 上处理mevan版本兼容问题的脚本（可删除 没影响）

- ** 没影响）

- cloud22020.iml 有的文件每个导入IDEA的项目都会生成一个项目同名的 .iml文件 用于保存你对这个项目的配置 （删了程序重新导入后还会生成 但由于配置丢失可能会造成程序异常）

- `.mvn` 目录：用于存放 Maven 项目的本地配置（如 `jvm.config`、`maven.config`）和 Maven Wrapper（版本管理）。

  `.mvn/wrapper` 目录：包含 Maven Wrapper 相关文件（`mvnw`、`mvnw.cmd`），确保项目使用固定版本的 Maven，可删除但影响自动管理。

