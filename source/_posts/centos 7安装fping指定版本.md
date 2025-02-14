---
layout: title
title: centos 7安装fping指定版本
date: 2023-11-06 22:14:36
tags: Linux
categories: Linux

---

# centos 7安装fping指定版本

 <!-- more -->

在CentOS 7上安装

fping 5.1可以通过源代码编译的方式进行。以下是安装步骤

1. 安装编译所需的依赖项：

```cmd
sudo yum install gcc make
```



2. **下载fping源代码：**

```cmd
wget https://fping.org/dist/fping-5.1.tar.gz
```



3. **解压源代码文件：**

```cmd
tar -xzvf fping-5.1.tar.gz 
cd fping-5.1
```



4. **编译和安装fping**

make之后可以到src目录下找fping执行**/usr/local/fping/fping-5.1/fping -v** 应该可以成功，

```cmd
./configure make

 sudo make install

make install
```

之后默认安装在/usr/local/sbin下

5. **给~/.bashrc和~/.bash_profile配置相关路径**

**~/.bashrc**

```cmd
vim ~/.bashrc 
添加 
export PATH=$PATH:/usr/local/sbin
重新加载 
source ~/.bashrc
```

配置如下：

```cmd
# .bashrc

# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi

export PATH=$PATH:/usr/local/sbin
```

**~/.bash_profile**

```cmd
vim ~/.bash_profile
添加
PATH=$PATH:$HOME/bin:/usr/local/sbin
重新加载
source ~/.bash_profile
```

配置如下：

```cmd
[sendi@zabbix1 ~]$ 
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/bin:/usr/local/sbin

export PATH
export JAVA_HOME=/usr/local/jdk1.8
export TOMCAT_HOME=/usr/local/tomcat
export CATALINA_HOME=/usr/local/tomcat
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CATALINA_HOME/bin
export CLASS_PATH=$JAVA_HOME/bin/lib:$JAVA_HOME/jre/lib:$JAVA_HOME/lib/tool.jar
export PATH
~           
```

  

，**~/.bashrc** 和 **~/.bash_profile** 是两个不同的文件，它们在 Bash Shell 中有不同的作用。

1. ~/.bashrc
   1. ~/.bashrc
   - 当用户每次打开一个新的终端窗口时，Bash Shell 会自动执行 ~/.bashrc
   - 通常，~/.bashrc 中包含用户特定的配置，如别名、环境变量等。
   - ~/.bashrc通常用于配置用户特定的 Shell 环境。

1. ~/.bash_profile
   1. ~/.bash_profile 是用户登录时 Bash Shell 的初始化文件。
   - 当用户登录时（通常是通过命令行登录或图形用户界面登录），Bash Shell 会自动执行 ~/.bash_profile 文件中的命令。
   - ~/.bash_profile 通常包含用户登录时需要进行的初始化操作，例如设置环境变量、启动程序等。
   - 一般来说，~/.bash_profile 只会在用户登录时执行一次，而不会在每次打开新终端窗口时执行。

在一些 Linux 系统中，**~/.bash_profile** 会引用 **~/.bashrc**，这样的话，**~/.bashrc** 中的配置会在每次打开新终端窗口时生效，而不仅限于用户登录时。

在你的情况下，如果你是希望在每次打开新终端窗口时都能够使用 **fping** 命令，你应该将相关的路径配置添加到 **~/.bashrc** 文件中。如果你希望只在用户登录时配置环境变量，那就将路径配置添加到 **~/.bash_profile** 文件中。

可能普通用户sudo fping www.baidu.com会执行不成功，显示sudo: fping: command not found

这可能是因为 sudo 命令的环境变量配置中没有包含 /usr/local/sbin 路径。

### **sudo** **的环境变量配置：**

编辑 **sudo** 的环境变量配置文件（通常是 **/etc/sudoers**）以确保 **/usr/local/sbin** 在 **sudo** 环境中被包含。不建议直接编辑 **/etc/sudoers** 文件，而是使用 **visudo** 命令来编辑，因为这样可以避免语法错误。

在终端中输入以下命令：

```cmd
sudo visudo
```

在打开的文件中，找到 

Defaults secure_path 行，确保这一行包含了 /usr/local/sbin 路径，例如：

```cmd
Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
```

如果这一行没有包含 /usr/local/sbin，请将其添加进去。保存并退出文件。

这样，**sudo** 命令就应该能够找到 fping 了，普通用户就可以使用 sudo 运行 fping 命令。如果在编辑 sudoers 文件时遇到问题，可能需要使用 **visudo 的 -f** 选项来编辑指定的文件，例如：

```cmd
sudo visudo -f /etc/sudoers
```

请注意，修改系统配置文件前请备份，确保不会引入错误。