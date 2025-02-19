---
title: linux常用命令（持续更新）
date: 2023-08-09 14:14:36
tags: Linux
categories: Linux
---

# linux常用命令（持续更新）

 <!-- more -->

```cmd

bin/kafka-consumer-groups.sh --new-consumer --zookeeper 172.53.81.33:2181 --topic aiops_warning --describe

#消费topic
bin/kafka-console-consumer.sh --zookeeper 172.53.81.33:2181 --topic aiops_warning --from-beginning |grep 'M_1040'> console.log
bin/kafka-console-consumer.sh --zookeeper 172.53.81.33:2181 --topic cloud_dept_ping_host_bus

#删除消费组
bin/kafka-consumer-groups.sh --zookeeper 172.53.81.33:2181 --group new-workplan-filter-test --delete

#查看消费组
bin/kafka-console-consumer.sh --bootstrap-server 172.53.81.33:9092 --list
bin/kafka-topics.sh --list --zookeeper 172.53.81.33:2181

#生产消息
bin/kafka-console-producer.sh --broker-list 172.53.81.11:9092 --topic cloud_dept_ping_host_bus
bin/kafka-console-producer.sh --broker-list 172.168.201.25:9092 --topic cloud_dept_ping_host_bus

#查看集群节点权重
bin/kafka-topics.sh --zookeeper 172.53.81.33:2181 --describe  | grep 'itmp_zabbix_data'


#跨服务器发送文件
scp -P目标服务器端口 文件名 用户名@目标ip:目标文件夹
scp -P58022 程序 sdnmuser@172.53.81.13:/home/sdnmuser/upload-file
d=MG_l){2cc

#跨服务器拉拉取文件
scp -P58022 sdnmuser@源服务器IP:/path/to/源服务器文件 /目标服务器目录



sftp  sdnmuser@132.122.150.216 输密码
get 文件名

#sdnmuser密码：9LE1g#HnpEQd(E
#dxwgwh30密码：l8@rxWXt0w93kp


ps -ef | grep zabbix_agent | grep -v grep  | awk -F ' ' '{print $2}' | xargs kill -9 
#重启zabbix_agent
sudo systemctl stop zabbix-agent
/home/sdnmuser/zabbix_agent/sbin/zabbix_agentd -c /home/sdnmuser/zabbix_agent/zabbix_agentd.conf
sudo systemctl start zabbix-agent

#重启zabbix_server
sudo systemctl stop zabbix-server
sudo systemctl start zabbix-server
sudo systemctl restart zabbix-server


#通过进程号找进程位置
ls -l /proc/<pid>/exe

#通过端口找进程号
sudo lsof -i:<port> -t

#启动prometheus 
nohup ./prometheus --config.file=prometheus.yml > /dev/null &

#修改分组

#删除当前目录小于1k的文件
find ./ -type f  -maxdepth 1 -size -1k -exec rm -f {} \;

ip a #就是网卡名
#设置丢包率
sudo tc qdisc add dev ens192 root netem loss 50%
#解除丢包率
sudo tc qdisc del dev ens192 root


#执行
ssh sdnmuser@132.96.28.8 'sh -s' < test_sybase.sh


# 设置iptables，放行80端口和22端口
iptables -I INPUT -p tcp --dport 80 -m state --state NEW -j ACCEPT
iptables -I INPUT -p tcp --dport 22 -m state --state NEW -j ACCEPT

sudo iptables -nL
sudo vim /etc/sysconfig/iptables
sudo service iptables restart

# INPUT链拒绝所有请求
iptables -P INPUT DROP
```

