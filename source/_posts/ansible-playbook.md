---
layout: title
title: ansible-playbook
date: 2024-12-01 21:14:36
tags: ansbile
categories: ansible

---

# ansible-playbook

<!-- more -->

**1.playbook的相关知识**

**1.1 playbook 的简介**

playbook是 一个不同于使用Ansible命令行执行方式的模式，其功能更强大灵活。简单来说，playbook是一个非常简单的配置管理和多主机部署系统，不同于任何已经存在的模式，可作为一个适合部署复杂应用程序的基础。Playbook可以定制配置，可以按照指定的操作步骤有序执行，支持同步和异步方式。我们完成一个任务，例如安装部署一个httpd服务，我们需要多个模块（一个模块也可以称之为task）提供功能来完成。而playbook就是组织多个task的容器，他的实质就是一个文件，有着特定的组织格式，它采用的语法格式是YAML（Yet Another Markup Language）。

**1.2 playbook的 各部分组成**

（1）Tasks：任务，即通过 task 调用 ansible 的模板将多个操作组织在一个 playbook 中运行

（2）Variables：变量

（3）Templates：模板

（4）Handlers：处理器，当changed状态条件满足时，（notify）触发执行的操作

（5）Roles：角色

示例：

![image-20250207225309226](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20250207225309226.png)

2. ## 基础的playbook剧本编写实例

**playbook中运用的模块就是ansible中的模块，就像docker-compose一样将docker操作容器的指令归纳为一个yaml文件，开启运行yaml中的指令模块就能按照预设计的方向去完成。 **

**实例1：playbook编写 apache的yum安装部署剧本**

剧本编写实现的需求：对Ansible管理的所有的webservers组的成员，yum安装最新版本的apache服务软件，并进行相应环境的调整，确保webservers的apache服务能够正常运行并设置开机自启

```
cd /etc/ansible    #在ansible的所在目录中创建该项目的目录
mkdir apache
vim apache.yaml
---
- name: apache yum apply
  gather_facts: false
  hosts: webservers
  remote_user: root
  tasks:
   - name: test connection
     ping:
 
   - name: stop firewalld
     service: name=firewalld state=stopped
 
   - name: stop selinux
     command: '/usr/sbin/setenforce 0'
     ignore_errors: true
 
   - name: yum install apache service
     yum: name=httpd state=latest
 
   - name: start apache service
     service: name=httpd state=started enabled=yes
```

![image-20250207225401011](C:\Users\JIA\AppData\Roaming\Typora\typora-user-images\image-20250207225401011.png)

**运行剧本的方法：**

```
//运行playbook
ansible-playbook apache.yaml
//携带参数，并行的级别 是10
ansible-playbook playbook.yml -e "" -f 10
 
 
 
 
 
 
//补充参数：
-k（–ask-pass）：用来交互输入ssh密码
-K（-ask-become-pass）：用来交互输入sudo密码
-u：指定用户
ansible-playbook apache.yaml --syntax-check    #检查yaml文件的语法是否正确
ansible-playbook apache.yaml --list-task       #检查tasks任务
ansible-playbook apache.yaml --list-hosts      #检查生效的主机
ansible-playbook apache.yaml --start-at-task='install httpd'     #指定从某个task开始运行
```

**注意：**

**1. ****在 Ansible 中，每个任务都在独立的 shell 中运行，命令只影响当前任务，不会影响其他任务**

例如：如果你想cd /data，sh test.sh，你这样写

```
---
- hosts: group2
  remote_user: root
  gather_facts: no
  become: no
  become_user: root
  become_method: sudo
  tasks:
    - name: cd /data
      command: cd /data
      register: cd_result
    - debug: 
        var: cd_result
      ignore_errors: yes

    - name: sh test.sh
      shell: "sh test.sh"
      register: test_sh_result
    - debug: 
        var: test_sh_result.stdout
```

是执行不成功的，因为第二个任务所在路径是根路径，不会因为第一个任务而改变

可以使用shell模块的`|`符号执行两条连贯的命令

```
---
- hosts: group2
  remote_user: root
  gather_facts: no
  become: no
  become_user: root
  become_method: sudo
  tasks:
    - name: Change to /data and execute test.sh
      shell: |
        cd /data
        sh test.sh
```

**在这个 Playbook 中，****shell**** 模块使用了 ****|**** 符号，它允许你在多行中编写 Shell 命令。这样，****cd /data**** 和 ****sh test.sh**** 两个命令都在同一个 Shell 中执行。**

**2. command模块和shell模块依赖目标主机的python，如果目标主机没装python，可以用raw（执行命令），script（执行脚本）**

**3.关于inventory文件**

对于需要登录上去之后切换其他用户，新版本使用ansible_become， ansible_become_user, ansible_become_pass，对每一台设置是否需要切换，以及切换的用户名密码，旧版本使用ansible_sudo_pass

```
别名（不能使用#连接ip，会被默认为主机ip）  ansible_ssh_host=172.168.201.32  ansible_ssh_user=user1  ansible_ssh_pass='passwd' ansible_become=true ansible_become_user=user2  ansible_become_pass='passwd'
```

