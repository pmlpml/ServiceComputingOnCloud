---
layout: default
title: 服务计算-操作实践
---

## 五、安装 docker 容器服务
{:.no_toc}

容器是微服务的最佳载体。因此，需要安装带 docker 虚拟机模板，以方便安装未来系统与应用的主机。

* 目录
{:toc}

### 1、准备虚拟机

使用链接方式在 centos-base 基础啊上创建 centos-docker-251 虚拟机

启动 centos-docker-251，使用 nmtui 将第二块网卡设置为 192.168.56.251。

#### 1.1 关闭防火墙并安装 iptables 服务

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

#### 1.2 禁用 SELINUX 关闭 swap

禁用SELINUX：

```
setenforce 0
```

修改 `/etc/selinux/config` 添加 `SELINUX=disabled`

```
vi /etc/selinux/config
SELINUX=disabled
```

关闭系统的Swap方法如下:

```
swapoff -a
```

修改 `/etc/fstab` 文件，注释掉 SWAP 的自动挂载，使用`free -m`确认swap已经关闭

```
vi /etc/fstab
# /dev/mapper/centos-swap swap 。。。
```

#### 1.3 启动 IP Forward

首先要打开IP转发功能

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

### 2、安装 Docker

安装docker的yum源:

```
yum install -y yum-utils device-mapper-persistent-data lvm2

yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

查看最新的 Docker 版本

```
yum list docker-ce.x86_64  --showduplicates |sort -r
Loading mirror speeds from cached hostfile
 * extras: mirrors.aliyun.com
docker-ce.x86_64            3:18.09.0-3.el7                     docker-ce-stable
docker-ce.x86_64            18.06.1.ce-3.el7                    docker-ce-stable
docker-ce.x86_64            18.06.0.ce-3.el7                    docker-ce-stable
docker-ce.x86_64            18.03.1.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            18.03.0.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.12.1.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.12.0.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.09.1.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.09.0.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.06.2.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.06.1.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.06.0.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.03.3.ce-1.el7                    docker-ce-stable
docker-ce.x86_64            17.03.2.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.03.1.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.03.0.ce-1.el7.centos             docker-ce-stable
 * base: mirrors.163.com
```

尽管有很多选择，请 **选择 `Docker 17.03.2` 版本**，除非部分容器集群明确要求其他版本！！！

```
yum install -y --setopt=obsoletes=0 \
   docker-ce-17.03.2.ce-1.el7.centos.x86_64 

systemctl start docker
systemctl enable docker
```

确认一下iptables filter表中FOWARD链的默认策略(pllicy)为ACCEPT。

```
iptables -nvL
Chain INPUT (policy ACCEPT 263 packets, 19209 bytes)
 pkts bytes target     prot opt in     out     source               destination
...  ...

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
    0     0 DOCKER-ISOLATION  all  --  *      *       0.0.0.0/0            0.0.0.0/0
    0     0 DOCKER     all  --  *      docker0  0.0.0.0/0            0.0.0.0/0
```

### 3、测试 Docker

使用 `docker version` 检查版本

```
docker version
Client:
 Version:      17.03.2-ce
 API version:  1.27
 Go version:   go1.7.5
 Git commit:   f5ec1e2
 Built:        Tue Jun 27 02:21:36 2017
 OS/Arch:      linux/amd64

Server:
 Version:      17.03.2-ce
 API version:  1.27 (minimum version 1.12)
 Go version:   go1.7.5
 Git commit:   f5ec1e2
 Built:        Tue Jun 27 02:21:36 2017
 OS/Arch:      linux/amd64
 Experimental: false
```

使用 `docker info` 检查，应该没有任何 warning

```
docker info
... ...
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: false
```

下载并运行 hello-world

```
docker run --rm hello-world
```

【参考】：

1. [官网指南](https://docs.docker.com/install/linux/docker-ce/centos/#install-docker-ce)
2. [cnetos7 安装 docker17.03.2](http://www.cnblogs.com/freefei/p/9263998.html)