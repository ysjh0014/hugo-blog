---
title: "在Centos中配置DNS服务"
date: 2018-11-28 19:51:35
draft: false
---
DNS（Domain Name System域名系统）

是一种基于分布式的数据库系统，并采用C/S模式进行主机域名与IP地址之间的转换

环境：

centos系统主机一台 IP地址：192.168.220.137

DNS软件包bind

步骤一：

查看是否安装DNS软件包bind
rpm -qa|grep bind

如果没有安装则安装

yum install bind

启动named服务，并设置为开机自启

systemctl start named systemctl enable named

步骤二：

配置文件

1.编辑/etc/named.conf
options { listen-on port 53 { any; }; allow-query { any; }; };

2.编辑/etc/named.rfc1912.zones

在最后添加下面内容
zone "ysjh.com" IN { type master; file "named.ysjh.com.zones"; allow-update { none; }; }; zone "220.168.192.in-addr.arpa" IN { type master; file "named.220.168.192.zones"; allow-update { none; }; };

3.编辑并创建/var/named/named.ysjh.com.zones

$TTL 1D @ IN SOA @ root.ys1.ysjh.com. ( 0 ; serial 1D ; refresh 1H ; retry 1W ; expire 3H ) ; minimum IN NS ys1.ysjh.com. ys1 IN A 192.168.220.137 ys2 IN A 192.168.220.138

4.编辑并创建/var/named/named.220.168.192.zones

$TTL 1D @ IN SOA @ root.ys1.ysjh.com. ( 0 ; serial 1D ; refresh 1H ; retry 1W ; expire 3H ) ; minimum IN NS ys1.ysjh.com. 137 IN PTR ys1.ysjh.com. 138 IN PTR ys2.ysjh.com.

步骤三：

重启DNS服务
systemctl restart named

修改主机名

hostnamectl set-hostname ys1

编辑/etc/resolv.conf

将其中的nameserver修改为
nameserver 192.168.220.137

编辑/etc/NetworkManager/NetworkManager.conf

在[main]下加上
dns=none

步骤四：

重启系统或者重启网卡

步骤五：

下载测试软件
yum install bind-utils

测试

nslookup ys1.ysjh.com nslookup ys2.ysjh.com