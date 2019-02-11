---
layout: default
title: 服务计算-操作实践
---

## 五、安装 desktop 桌面 GNOME
{:.no_toc}

桌面不是一个必须的选项（如果你熟悉 VIM 等），但是考虑到图形界面的便利，如用做 go 编程等，安装一个 Linux 桌面是必须的选择。

* 目录
{:toc}

### 1、准备虚拟机

使用链接方式在 centos-base 基础啊上创建 centos-desktop-100 虚拟机

设置该虚拟机 CPU 至少 2 ，内存至少 4096 M，显存设为 128M

启动 centos-desktop-100，使用 nmtui 将第二块网卡设置为 192.168.56.100。

#### 1.1 安装桌面组件

升级核心

```
yum update
```

安装图形桌面

```
yum groupinstall "GNOME Desktop"
```

请耐心等待下载与安装，直到完毕！

#### 1.2 安装构建 Linux 内核工具

由于安装 VirtualBox 扩展组件需要编译内核，必须安装以下工具：

添加 EPEL 仓库：

```
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

安装相关包

```
yum install perl gcc dkms kernel-devel kernel-headers make bzip2
```

查看相关头文件

```
ls -l /usr/src/kernels/$(uname -r)
```

#### 1.3 设置图形启动 

在root用户权限下，设置centos系统默认的启动方式，输入命令如下

```
systemctl set-default multi-user.target  //设置成命令模式
systemctl set-default graphical.target  //设置成图形模式
```

重启

### 2 安装 VirtualBox Guest Additions

首先按向导重新建立桌面用户名和密码。（注意，桌面一般运行在普通用户状态，**不宜使用 root**）

在 VirtualBox 的菜单 设备 - 添加增强功能，挂载 Additions ISO盘。 在 Additions ISO盘，按鼠标右键，打开终端，运行

```
sudo ./autorun.sh
```

经过一段时间编译，完成后重启！


【参考】：

1. [CentOS as a Guest OS in VirtualBox](https://wiki.centos.org/HowTos/Virtualization/VirtualBox/CentOSguest)
2. [Installing VirtualBox Guest Additions on CentOS 7](https://sobo.red/2017/04/27/installing-virtualbox-guest-additions-on-centos-7/)