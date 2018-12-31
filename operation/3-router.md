---
layout: default
title: 服务计算-操作实践
---

## 三、设置路由网关服务
{:.no_toc}

部署虚拟机集群，需要一组都能上网的虚拟机（VM），可是我们在特定场合下只有一个上网的地址。
本节的任务是 用一个 VM 在 VirualBox 创建的虚拟网络和主机网络之间构建 NAT，使得虚拟网络上的 VM 都可以上网。

（如果你的主机是 Linux，请在主机上设置 NAT 将虚拟网络流量转发到网卡）

* 目录
{:toc}

### 准备路由虚拟机

使用链接方式在 centos-base 基础啊上创建 contos-router-254 虚拟机

启动 contos-router-254，使用 nmtui 将第二块网卡设置为 192.168.56.254。

### 1、关闭防火墙并安装 iptables 服务

检查防火墙服务

```
# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since 一 2018-12-31 11:21:29 CST; 25min ago
```

关闭防火墙

```
systemctl stop firewalld
systemctl disable firewalld
systemctl mask firewalld
```

同时安装 iptables

```
yum install iptables-services

systemctl enable iptables && systemctl enable ip6tables
systemctl start iptables && systemctl start ip6tables
```

### 2、检查网卡配置

```
# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:8d:0c:77 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 84340sec preferred_lft 84340sec
    inet6 fe80::ad3f:d4a4:d356:86dd/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:9d:36:8b brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.254/24 brd 192.168.56.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::5c91:1ae6:5d1:1bf9/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```

这里： enp0s3 是动态的，连外网；enp0s8 连内网，连内网（VirtualBox虚拟网络）

centos 的网卡的配置文件在 `/etc/sysconfig/network-scripts/` 目录下，你可以手动配置它们。

阅读这些配置文件，对于理解 Linux 文本驱动的理念，以及未来自动化配置非常有价值。修改配置文件后，`systemctl restart network.service` 就生效了。

### 3、配置iptables使可以访问外网

1、首先要打开IP转发功能

```
echo 1 > /proc/sys/net/ipv4/ip_forward 
```

由于每次重启网络，ip_forward就被重新设置成0,设置文件 `/etc/sysctl.conf`

```
net.ipv4.ip_forward = 1
```

使配置立即生效

```
sysctl -p
```

2、建立 nat 包装

修改数据报头信息：

```  
iptables -t nat -A POSTROUTING -o eth1 -j MASQUERADE 
或
iptables -t nat -A POSTROUTING -s 192.168.56.0/24 -o enp0s3 -j MASQUERADE
```

3、建立转发

```
iptables -A FORWARD -i enp0s8 -j ACCEPT
或
iptables -A FORWARD -s 192.168.56.0/24 -m state --state ESTABLISHED,RELATED -j ACCEPT
```

4、保存 iptables 配置

```
service iptables save
```

### 4、测试

设置完这些之后，我们复制一台新虚拟机，第一块网卡使用 Host-Only，取消其他网卡。

启动后，使用 nmtui 设置 enp0s3 为静态网址，设置 ip=192.168.56.253/24 , Gateway：192.168.56.254 ， DNS：223.5.5.5 。同时删除其他网卡配置

`ping 163.com` 不通， `ping 223.5.5.5` 提示 `From 192.168.56.254 ... Destination Host Prohibited `，网关不转发？

在网关服务器：

```
iptables -n -L
```

发现输出有

```
REJECT     all  --  0.0.0.0/0            0.0.0.0/0           reject-with icmp-host-prohib
```

打开 `service iptables save` 产生的路由规则文件 `/etc/sysconfig/iptables`

注释掉 `reject-with icmp-host-prohib` 相关的行，然后

```
reboot
```

在到测试虚拟机，网通了！


参考：

1. [Centos 7 网关服务器的配置](https://www.tianmaying.com/tutorial/centos7)
2. [centos7下搭建NAT和DHCP服务器](https://blog.csdn.net/qq_41684957/article/details/81533977)