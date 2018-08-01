---
layout: default
title: Go 开发环境安装
---

# 安装 go 语言开发环境
{:.no_toc}

* 目录
{:toc}

既然选择后台开发，自然建议你在 Linux 环境下安装 go 语言开发环境。这里仅是 centos 7 安装的部分内容。

## 1、安装 VSCode 编辑器

![](https://pmlpml.github.io/unity3d-learning/images/drf/info.png) 如果你是 vim 或 emacs 用户，可以忽略以下内容。

如果你曾经是 Notepad++ 或 Sublime text 或 Atom 的用户，你不得不考虑改用微软 VSCode 做轻量级的编程。 JavaScript 技术，兼容几乎所有流行的操作系统，特别是对中文支持堪称完美！它不仅是跨平台多语言软件开发工具，而且是 Linux 平台写 [Github Flavored Markdown](https://github.github.com/gfm/) 的神器。[官方介绍](https://code.visualstudio.com/docs)：

> Visual Studio Code 是一个轻量级但功能强大的源代码编辑器，可在 Windows，macOS 和 Linux 桌面上运行。它内置了对JavaScript，TypeScript和Node.js的支持，并为其他语言（如C ++，C＃，Java，Python，PHP，Go）和运行时（如.NET和Unity）提供了丰富的扩展生态系统。

linux 下安装:

* [Running VS Code on Linux](https://code.visualstudio.com/docs/setup/linux)

## 2、安装 golang

Golang [官方网站](https://golang.org) 提供了不同平台的安装。可是 ... ...

golang [中国项目组](https://go-zh.org/) 提供了近可能好的中文服务。如果你有兴趣，发现问题可联系它们，使得中文服务变得更加完善。

### 3.1 安装

中文安装指南位置：https://golang.org/doc/install。 然而 ... ... 链接的二进制发行文件呢？

用你的包管理工具，以 CentOS 7 为例：

```
$ sudo yum install golang
```

安装到哪个目录了呢？

```
$ rpm -ql golang |more
```

测试安装：

```
$ go version
```
 
### 3.2 设置环境变量

go 对编译、包管理、测试、部署、运行提供全程支持，了解**环境配置**非常重要！

[go 语言工作空间](https://go-zh.org/doc/code.html)

**1、创建工作空间**

```
$ mkdir $HOME/gowork
```

**2、配置的环境变量**，对于 centos 在 `~/.profile` 文件中添加:

```
export GOPATH=$HOME/gowork
export PATH=$PATH:$GOPATH/bin
```

然后执行这些配置

```
$ source $HOME/.profile
```

**3、检查配置**

```
$ go env
...
GOPATH = ...
...
GOROOT = ...
...
```

### 3.3 创建 hello world！

**请退出当前用户，然后重新登陆！！！**

创建源代码目录：

```
$ mkdir $GOPATH/src/github.com/github-user/hello -p
```

使用 vs code 创建 hello.go

```go
package main

import "fmt"

func main() {
    fmt.Printf("hello, world\n")
}
```

在终端运行!

```
$ go run hello.go
hello, world
```


## 4、安装必要的工具和插件

### 4.1 安装 Git 客户端

go 语言的插件主要在 Github 上，安装 git 客户端是首要工作。

```
$ sudo yum install git
```

### 4.2 安装 go 的一些工具

进入 vscode ，它提示要安装一些工作，但 ... 悲剧发生了 `failed to install.`

仔细检查，发现 `https://golang.org/x/tools/...` ， emmm  原来 golang.org 连不上！

**1、下载源代码到本地**

```
# 创建文件夹
mkdir $GOPATH/src/golang.org/x/
# 下载源码
go get -v github.com/golang/tools
# copy 
cp $GOPATH/src/github.com/golang/tools $GOPATH/src/golang.org/x/ -rf
```

**2、安装工具包**

```
go install golang.org/x/tools/go/buildutil
```

退出 vscode，再进入，按提示安装！

![](https://pmlpml.github.io/unity3d-learning/images/drf/info.png) 查看 go 当前工作空间的目录结构，应该和官方文档 [如何使用Go编程](https://go-zh.org/doc/code.html) 的工作空间一致

细节参考： [获取Golang.org上的Golang Packages](https://github.com/northbright/Notes/blob/master/Golang/china/get-golang-packages-on-golang-org-in-china.md)

**3、安装运行 hello world**

```
$ go install github.com/github-user/hello
$ hello
```

## 5、安装与运行 go tour

细节参见：[《Go 语言之旅》](https://github.com/Go-zh/tour)

```
$ go get github.com/Go-zh/tour/gotour
$ gotour
```



