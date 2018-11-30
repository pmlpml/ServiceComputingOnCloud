---
layout: default
title: web 服务构建 - gRPC
---

# Golang web 服务构建 - gRPC 
{:.no_toc}

随着技术的发展，人们也不再认为 REST 是构建 web 服务的唯一选择。RPC 风格、 REST 风格、 GraphQL API 的 web 服务将在很长一段时间内共存（应该是各自取长补短，各守一片天地，甚至在同一应用中，不同风格 API 混搭的局面）。本文选取 gRPC、swagger、GraphQL作为服务 API 描述标准，简单介绍三种 API 的服务构建（go 语言）。

## gRPC ？

## hello world

gRPC 官当文档 [hello world](https://grpc.io/docs/quickstart) 质量非常好，只是因为“墙”和版本更新，网上许多博客可能会困惑你。目前最新版本 Protocol Buffers v3。

### 1、准备工作 

**1.1 检查 go 版本**

gRPC 要求 go 1.6 或以上

```bash
$ go version
```

**1.2 安装 gRPC**

官方命令

```
$ go get -u google.golang.org/grpc
```

结果是超时错误！！！`package google.golang.org/grpc: unrecognized import path "google.golang.org/grpc" (https fetch: Get https://google.golang.org/grpc?go-get=1: dial tcp 216.239.37.1:443: i/o timeout)`

按规矩到 github 找  `https://github.com/google/grpc` 发现 `https://github.com/google/grpc` 有各种版本实现。于是：

```
$ go get -d github.com/grpc/grpc-go
```

下载完成后，需要将 `$GOPATH/src/github.com/grpc/grpc-go` 的内容复制到 `$GOPATH/src/google.golang.org/grpc`

```
$ mkdir $GOPATH/src/google.golang.org/grpc -p
$ cp $GOPATH/src/github.com/grpc/grpc-go/* $GOPATH/src/google.golang.org/grpc -rf
$ go install google.golang.org/grpc/
```

然而，在 install grpc 时一堆包找不到 ... 包括：`github.com/golang/protobuf/...`,`golang.org/x/net/...`,`golang.org/x/sys/...`,`google.golang.org/genproto/...`

然后，凡是 `golang.org` 上的包都需要从 `github.com/golang` 或 `github.com/google` 上下载。例如：

```
$ go get -d github.com/golang/net
$ cp $GOPATH/src/github.com/golang/net $GOPATH/src/golang.org/x/net -rf
... 用同样方法下载 github.com/golang/sys 和 github.com/golang/text
$ go get -d github.com/golang/protobuf
$ go get -d github.com/google/go-genproto
$ cp $GOPATH/src/github.com/google/go-genproto/* $GOPATH/src/google.golang.org/genproto -rf
$ go install google.golang.org/grpc/
```

总算安装完 go 版本的 grpc！

**1.3 安装 Protocol Buffers v3**

安装 protoc 编译器用于产生 gRPC 服务代码。最简单的方法是下载预编译的二进制版本（protoc-\<version\>-\<platform\>.zip）。下载地址：https://github.com/google/protobuf/releases。例如：

```
$ wget https://github.com/protocolbuffers/protobuf/releases/download/v3.6.1/protoc-3.6.1-linux-x86_64.zip
```

* 解压文件
* 修改环境变量 PATH 包含 protoc/bin 目录。

最后，安装 protoc 的 go 插件

```
$ go get github.com/golang/protobuf/protoc-gen-go
```

### 2、案例体验

进入产品的案例目录：

```
$ cd $GOPATH/src/google.golang.org/grpc/examples/helloworld
```

这是一个已生成好的案例。 在 `helloworld` 有 `helloworld.proto` 接口定义文件 和 由它生成的 `helloworld.pb.go`。 `.pb.go` 包含：

* 生成的客户端和服务器代码
* 消息序列化、反序列化等代码

**试一试**

在终端运行服务器：

```
$ go run greeter_server/main.go
```

在另一个终端运行:

```
$ go run greeter_client/main.go
2018/11/23 17:00:16 Greeting: Hello world
```

### 3. 编写自己的 hello world

**3.1 创建项目目录**

在自己的空间创建 helloworld 项目。并在项目下创建三个子目录：

* def 保存定义 与 protoc 生成的代码
* server 服务器代码
* client 客户端代码

**3.2 编写接口定义文件**

在 def 下创建文件 `helloword.proto`

```
// The greeting service definition.
syntax = "proto3";

service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {};
  // Sends another greeting
  rpc SayHelloAgain (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings
message HelloReply {
  string message = 1;
}
```

这是一个及其类似 c 语言的定义

### 3.3 产生代码

在 helloworld 目录打开一个命令终端：

```
protoc -I def/ def/helloworld.proto --go_out=plugins=grpc:def
```

正常就产生了 `helloworld.pb.go`

### 3.4 修改应用程序

拷贝原服务器 main.go 到 server 目录，修改：

* `pb` 包别名为 `.pb.go` 所在的目录。例如：`github.com/.../grpc/helloworld/def`
* 添加服务器实现代码

```go
func (s *server) SayHelloAgain(ctx context.Context, in *pb.HelloRequest) (*pb.HelloReply, error) {
	return &pb.HelloReply{Message: "Hello again " + in.Name}, nil
}
```

实现 GreeterServer 接口。

拷贝原客户端 main.go 到 client 目录，修改：

* `pb` 包别名为 `.pb.go` 所在的目录。例如：`github.com/.../grpc/helloworld/def`
* 添加客户端调用代码

```go
	r, err = c.SayHelloAgain(ctx, &pb.HelloRequest{Name: name})
	if err != nil {
		log.Fatalf("could not greet: %v", err)
	}
	log.Printf("Greeting: %s", r.Message)
```

运行服务器：

```
$ go run server/main.go
```

运行客户端：

```
$ go run client/main.go
2018/11/23 18:25:54 Greeting: Hello world
2018/11/23 18:25:54 Greeting: Hello again world
```

项目源代码地址： https://github.com/pmlpml/golang-learning/tree/master/services/grpc/helloworld
