---
title: ansible-ad-hoc以及模块
date: 2024-10-14 21:14:36
tags: ansible
categories: ansible
---

# ansible-ad-hoc以及模块

<!-- more -->

# **Ansible功能介绍**

 批量执行远程命令,可以对远程的多台主机同时进行命令的执行批量安装和配置软件服务，可以对远程的多台主机进行自动化的方式配置和管理各种服务编排高级的企业级复杂的IT架构任务, Ansible的Playbook和role可以轻松实现大型的IT复杂架构提供自动化运维工具的开发API, 有很多运维工具,如jumpserver就是基于 ansible 实现自动化管理功能

## **ansible的特点**

- 模块化：调用特定的模块完成特定任务，支持自定义模块，可使用任何编程语言写模块

- Paramiko（python对ssh的实现），PyYAML，Jinja2（模板语言）三个关键模块

- 基于Python语言实现

- 部署简单，基于python和SSH(默认已安装)，agentless，无需代理不依赖PKI（无需ssl）

- 安全，基于OpenSSH

- 幂等性：一个任务执行1遍和执行n遍效果一样，不因重复执行带来意外情况,此特性非绝对

- 支持playbook编排任务，YAML格式，编排任务，支持丰富的数据结构

- 较强大的多层解决方案 role


## **Ansible结构介绍**

**Ansible 组成**

- 组合INVENTORY、API、MODULES、PLUGINS的绿框，为ansible命令工具，其为核心执行工具

- INVENTORY：Ansible管理主机的清单/etc/anaible/hosts

- MODULES：Ansible执行命令的功能模块，多数为内置核心模块，也可自定义

- PLUGINS：模块功能的补充，如连接类型插件、循环插件、变量插件、过滤插件等，该功能不常用

- API：供第三方程序调用的应用程序编程接口


**Ansible命令执行的来源**

- USER 普通用户，即SYSTEM ADMINISTRATOR

- PLAYBOOKS：任务剧本（任务集），编排定义Ansible任务集的配置文件，由Ansible顺序依次执行，通常是JSON格式的YML文件

- CMDB（配置管理数据库） API 调用

- PUBLIC/PRIVATE CLOUD API调用

- USER-> Ansible Playbook -> Ansibile


**Ansible的注意事项**

- 执行ansible的主机一般称为管理端, 主控端，中控，master或堡垒机

- 主控端Python版本需要2.6或以上

- 被控端Python版本小于2.4，需要安装python-simplejson

- 被控端如开启SELinux需要安装libselinux-python

- windows 不能做为主控端


**inventory 主机清单文件**

 ansible的主要功用在于批量主机操作，为了便捷地使用其中的部分主机，可以在inventory 主机清单文件中将其分组组织默认的inventory file为 /etc/ansible/hostsinventory file可以有多个，且也可以通过Dynamic Inventory来动态生成。

注意: 生产建议在每个项目目录下创建项目独立的hosts文件

**官方文档**

https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html

**Inventory 参数说明**

```
ansible_ssh_host             #将要连接的远程主机名.与你想要设定的主机的别名不同的话,可通过此变量设置.
ansible_ssh_port             #ssh端口号.如果不是默认的端口号,通过此变量设置.这种可以使用 ip:端口
 192.168.1.100:2222
ansible_ssh_user             #默认的 ssh 用户名
ansible_ssh_pass              #ssh 密码(这种方式并不安全,我们强烈建议使用 --ask-pass 或 SSH 密钥)
ansible_sudo_pass               #sudo 密码(这种方式并不安全,我们强烈建议使用 --ask-sudo-pass)
ansible_sudo_exe (new in version 1.8) #sudo 命令路径(适用于1.8及以上版本)
ansible_connection             #与主机的连接类型.比如:local, ssh 或者 paramiko. Ansible 1.2 以前默
认使用 paramiko.1.2 以后默认使用 'smart','smart' 方式会根据是否支持 ControlPersist, 来判断'ssh' 方式是否可行.
ansible_ssh_private_key_file #ssh 使用的私钥文件.适用于有多个密钥,而你不想使用 SSH 代理的情况.
ansible_shell_type         #目标系统的shell类型.默认情况下,命令的执行使用 'sh' 语法,可设置为'csh' 或 'fish'.
ansible_python_interpreter #目标主机的 python 路径.适用于的情况: 系统中有多个 Python, 或者命令路径不是"/usr/bin/python", 比如 \*BSD, 或者 /usr/bin/python 不是 2.X 版本的Python.之所以不使用 "/usr/bin/env" 机制,因为这要求远程用户的路径设置正确,且要求 "python" 可执行程序名不可为 python以外的名字(实际有可能名为python26).与ansible_python_interpreter 的工作方式相同,可设定如 ruby 或 perl 的路径....
```

**范例：**

```
[test]
10.0.0.8 ansible_connection=local #指定本地连接,无需ssh配置
#ansible_connection=ssh 需要StrictHostKeyChecking no
10.0.0.7 ansible_connection=ssh ansible_ssh_port=2222 ansible_ssh_user=wang
ansible_ssh_password=magedu
10.0.0.6 ansible_connection=ssh ansible_ssh_user=root
ansible_ssh_password=123456
#执行ansible命令时显示别名,如web01
[websrvs]
web01 ansible_ssh_host=10.0.0.101
web02 ansible_ssh_host=10.0.0.102
[websrvs:vars]
ansible_ssh_password=magedu
some_host ansible_ssh_port=2222 ansible_ssh_user=manager
aws_host ansible_ssh_private_key_file=/home/example/.ssh/aws.pem
freebsd_host ansible_python_interpreter=/usr/local/bin/python
ruby_module_host ansible_ruby_interpreter=/usr/bin/ruby.1.9.3

```

## **Ansible相关工具**

/usr/bin/ansible 主程序，临时命令执行工具

/usr/bin/ansible-doc 查看配置文档，模块功能查看工具,相当于man

/usr/bin/ansible-playbook 定制自动化任务，编排剧本工具,相当于脚本 /usr/bin/ansible-pull 远程执行命令的工具

/usr/bin/ansible-vault 文件加密工具

/usr/bin/ansible-console 基于Console界面与用户交互的执行工具 /usr/bin/ansible-galaxy 下载/上传优秀代码或Roles模块的官网平台

## **利用ansible实现管理的主要方式：**

Ansible Ad-Hoc 即利用ansible命令，主要用于临时命令使用场景 Ansible

playbook 主要用于长期规划好的，大型项目的场景，需要有前期的规划过程

## **ansible-doc：获取指定模块的帮助文档**

**格式：**

```
ansible-doc [options] [module...]

-l, --list #列出可用模块

-s, --snippet #显示指定模块的playbook片段
```

**列出指定模块帮助**

```
ansible-doc -s module_name
```

**ansible Ad-Hoc的 执行方式**

 **ansible Ad-Hoc说明：**

```
Ansible Ad-Hoc的执行方式主要的工具就是ansible命令:

#格式：ansible <host-pattern> [-m module_name] [-a args]

 ansible -i inventory -m command -a 'pwd' all
```

**​ 常用选项说明：**

```
显示版本--version

指定模块，默认为command-m module

详细过程 -vv -vvv更详细-v

显示主机列表，可简写 --list-list-hosts

检查，并不执行-C, --check

执行命令的超时时间，默认10s-T, --timeout=TIMEOUT

提示输入ssh连接密码，默认Key验证-k, --ask-pass

执行远程执行的用户,默认root-u, --user=REMOTE_USER

代替旧版的sudo 切换-b, --become

指定sudo的runas用户，默认为root--become-user=USERNAME

提示输入sudo时的口令-K, --ask-become-pass

指定并发同时执行ansible任务的主机数-f FORKS, --forks FORKS
```

**范例：调用shell模块查看shadow文件**

```
#修改被控制端的suduers文件，添加用户ALL=(ALL) NOPASSWD: ALL字段：
#sed -ri '/## Same thing without a password/a\xiang ALL =(ALL) NOPASSWD: ALL /etc/sudoers
#调用shell模块查看shadow
#ansible 192.168.213.122 -m shell -a 'sudo cat /etc/shadow |head -5' -u xiang -k -b
[root@Ansible-Master ~14:18]$ ansible 192.168.213.123 -m shell -a 'sudo cat /etc/shadow|head -5' -u xiang -k 
SSH password: 
[WARNING]: Consider using 'become', 'become_method', and 'become_user' rather than running sudo
192.168.213.123 | CHANGED | rc=0 >>
root:$6$nZpI0Gve/q3y/vLW$k8QocsCR4x65nEqLaFsHhQGR8eYibTP.ixE5pzNY.8qTz3OkwWWIs8c4Z5QZJ6VZ8Lv0YVNvVoapARSpAuHDm1::0:99999:7:::
bin:*:18353:0:99999:7:::
daemon:*:18353:0:99999:7:::
adm:*:18353:0:99999:7:::
lp:*:18353:0:99999:7:::

```

## **Aansible常用模块**

2015年底270多个模块，2016年达到540个，2018年01月12日有1378个模块，2018年07月15日1852 个模块,2019年05月25日（ansible 2.7.10）时2080个模块，2020年03月02日有3387个模块

参考文档

虽然模块众多，但最常用的模块也就2，30个而已，针对特定业务只用10几个模块 常用模块帮助文档参考：

https://docs.ansible.com/ansible/2.9/modules/modules_by_category.html

https://docs.ansible.com/ansible/2.9/modules/list_of_all_modules.html

https://docs.ansible.com/ansible/latest/modules/list_of_all_modules.html

https://docs.ansible.com/ansible/latest/modules/modules_by_category.html·



### **Command模块**

功能

在远程主机执行命令，此为默认模块，可忽略 -m 选项

注意：此命令不支持 $VARNAME < > | ; & 等，可能用shell模块实现

注意：此模块不具有幂等性

**常用参数**

- argv：将命令作为列表传输，而非字符串，且只能提供字符串或列表形式，不能同时提供两者，必须提供其中一种

- 
  chdir：运行命令前，切换到的目录

- 
  free_form参数 ：必须参数，指定需要远程执行的命令。需要说明一点，free_form 参数与其他参数（如果想要使用一个参数，那么则需要为这个参数赋值，也就是name=value模式）并不相同。比如，当我们想要在远程主机上执行 ls 命令时，我们并不需要写成”free_form=ls” ，这样写反而是错误的，因为并没有任何参数的名字是 free_form，当我们想要在远程主机中执行 ls 命令时，直接写成 ls 即可。因为 command 模块的作用是执行命令，所以，任何一个可以在远程主机上执行的命令都可以被称为 free_form

- 
  creates:与removes互斥且功能相反，指定一个文件路径或者目录路径，当该目录或文件存在时，则命令不执行

- 
  removes：与creates互斥功能相反，指定一个文件路径或者目录路径，当该目录或文件存在时，则命令执行，改参数不会删除文件；

- 
  stdin：标准输入

- 
  stdin_add_newline：给 stdin的值增加换行符；默认为YES，增加换行符；


**范例：**

```
#查看远端passwd信息

ansible webservs -m command -a 'ls -l /etc/passwd'
```

**shell模块（实用）**

**功能**

- 和command相似，用shell执行命令,支持各种符号,比如:*,$, >

- 注意：此模块不具有幂等性

- 该模块采用命令名称，后跟一个空格分隔参数列表

- 自由格式的命令或参数是必需的

- 它几乎与命令模块完全相同，但通过远程节点上的 shell （） 运行命令：/bin/sh

- 对于 Windows 目标，请改用win_shell模块


**常用参数**

- chdir：运行命令前，切换到的目录

- creates:与removes互斥且功能相反，指定一个文件路径或者目录路径，当该目录或文件存在时，则命令不执行

- removes：与creates互斥功能相反，指定一个文件路径或者目录路径，当该目录或文件存在时，则命令执行

- executable：更改用于执行该命令的shell，必须为绝对路径；


**范例：**

```
#获取远端主机的主机名称

[root@Ansible-Master ~11:03]#ansible webservs -m shell -a "echo $HOSTNAME"
```

