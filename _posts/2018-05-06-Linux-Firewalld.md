---
layout: post
title: 'Linux 防火墙配置（firewalld）'
subtitle: 'Firewalld的基本操作'
date: 2018-05-06
author: Mia0Yang
cover: '/assets/img/Linux-Firewalld-01.png'
tags: Linux
---

>关于Firewalld及其基本操作的命令

### Firewalld

在CentOS7中，firewallD 是 iptables 的前端控制器,用于实现持久的网络流量规则，有图形界面和字符界面。firewalld加入了区域(zone)概念，设定的区域和网络接口关联起来才能有效。并且firewalld支持动态更新，不用重启服务或全部刷新。

事实上firewalld本身并不具有防火墙的功能，它的作用是维护规则，需要通过内核的netfilter来实现（和iptables一样），所以真正使用规则的是内核的netfilter。

### Firewalld的基本操作

 firewalld的配置文件以xml格式为主（主配置文件firewalld.conf例 外），主要有两个存储位置：
 1：/etc/firewalld/  由用户自定义配置
 2：/usr/lib/firewalld/ 通用配置文件，一般由系统配置 
  当需要一个规则文件时 firewalld 会首先到第 1 目录中查找  如果可以找到，就直接使用，否则会继续到第  2 目录中查找

firewalld默认提供了九个zone配置文件,位置：  /usr/lib/firewalld/zones 
* drop.xml: 任何传入的网络连接都被拒绝 
* block.xml: 任何传入的网络数据包都将被阻止 
* public.xml: 不相信网络上的任何计算机，选择性接受传入网络连接 
* dmz.xml: 非军事区域，内外网络之间增加的一层网络，起到缓冲 
* 作用。对于隔离区域，选择性接受传入网络连接 
* work.xml: 工作组，相信网络上的其他计算机 
* home.xml: 家庭组，相信网络上的其他计算机 
* internal.xml:信任网络上的其他计算机,选择性接受传入网络连接 
* external.xml:不相信网络上的其他计算机 
* trust.xml: 所有的网络连接都可以接

firewalld的基本操作命令如下：

1、安装firewalld：
```linux
# yum install firewalld firewall-config
```

2、运行、停止、禁用firewalld

启动:
```linux
# systemctl start  firewalld 
```

停止:
```linux
# systemctl disable firewalld   
```    
禁用:
```linux
# systemctl stop firewalld   
```       
查看状态:
```linux 
# systemctl status firewalld 或者 firewall-cmd --state 
```  
 
3、配置firewalld

firewall-cmd是firewalld的字符界面管理工具，

查看版本：
```linux
$ firewall-cmd --version
```
查看帮助：
```linux
$ firewall-cmd --help
```
显示状态：
```linux
$ firewall-cmd --state
```
查看区域信息:
```linux
$ firewall-cmd --get-active-zones
```
查看指定接口所属区域：
```linux
$ firewall-cmd --get-zone-of-interface=eth0
```
拒绝所有包：
```linux
# firewall-cmd --panic-on
```
取消拒绝状态：
```linux
# firewall-cmd --panic-off
```
查看是否拒绝：
```linux
$ firewall-cmd --query-panic
```
更新防火墙规则：
```linux
# firewall-cmd --reload
# firewall-cmd --complete-reload
```
两者的区别就是第一个无需断开连接，就是firewalld特性之一动态添加规则，第二个需要断开连接，类似重启服务

将接口添加到区域，默认接口都在public： 
```linux
# firewall-cmd --zone=public --add-interface=eth0
```
永久生效再加上 --permanent 然后reload防火墙

设置默认接口区域：
```linux
# firewall-cmd --set-default-zone=public
```
立即生效无需重启
 
打开端口：
    查看所有打开的端口：
    
```linux
# firewall-cmd --zone=dmz --list-ports
```
    加入一个端口到区域：
    
```linux
# firewall-cmd --zone=dmz --add-port=8080/tcp
```

开启伪装IP： 
```linux
#firewall-cmd --permanent --add-masquerade
```

配置端口转发： 
```linux
# firewall-cmd --permanent --zone=<ZONE> --add-richrule='rule family=ipv4 source address=192.168.122.0/24 forw ard-port port=80 protocol=tcp to-port=8080' 
```

配置FTP：
允许服务、80端口(开放10050到10060端口)访问网络: 
```linux
# firewall-cmd --zone=public --permanent --add-port=80/tcp  
# firewall-cmd --permanent --zone=public --add-service=samba  
# firewall-cmd --permanent --zone=public --add-port=1005010060/tcp 
# firewall-cmd --reload 
```

配置NAT：
```linux
# firewall-cmd --permanent --zone=<ZONE> --addmasquerade 
 
# firewall-cmd --permanent --zone=<ZONE> --add-richrule='rule family=ipv4 source address=192.168.122.0/24 mas querade'
```

<img src = "/assets/img/Linux-Firewalld-02.png">




