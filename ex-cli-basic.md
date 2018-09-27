## CLI 命令行实用程序开发基础

### 1、概述

&emsp;&emsp;CLI（Command Line Interface）实用程序是Linux下应用开发的基础。正确的编写命令行程序让应用与操作系统融为一体，通过shell或script使得应用获得最大的灵活性与开发效率。Linux提供了cat、ls、copy等命令与操作系统交互；go语言提供一组实用程序完成从编码、编译、库管理、产品发布全过程支持；容器服务如docker、k8s提供了大量实用程序支撑云服务的开发、部署、监控、访问等管理任务；git、npm等都是大家比较熟悉的工具。尽管操作系统与应用系统服务可视化、图形化，但在开发领域，CLI在编程、调试、运维、管理中提供了图形化程序不可替代的灵活性与效率。

### 2、基础知识

&emsp;&emsp;几乎所有语言都提供了完善的 CLI 实用程序支持工具。以下是一些入门文档（c 语言）：

* [开发 Linux 命令行实用程序](https://www.ibm.com/developerworks/cn/linux/shell/clutil/index.html)。
* [Linux命令行程序设计](https://wenku.baidu.com/view/c7cf91ee5ef7ba0d4a733b58.html)

如果你熟悉 python ：

* [Using Python to create UNIX command line tools](https://www.ibm.com/developerworks/aix/library/au-pythocli/index.html)

阅读以后你应该知道 POSIX/GNU 命令行接口的一些概念与规范。命令行程序主要涉及内容：

* 命令
* 命令行参数
* 选项：长格式、短格式
* IO：stdin、stdout、stderr、管道、重定向
* 环境变量

### 3、Golang 的支持

使用os，flag包，最简单处理参数的代码：

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    for i, a := range os.Args[1:] {
        fmt.Printf("Argument %d is %s\n", i+1, a)
    }

}
```

使用flag包的代码：

```go
package main

import (
    "flag" 
    "fmt"
)

func main() {
    var port int
    flag.IntVar(&port, "p", 8000, "specify port to use.  defaults to 8000.")
    flag.Parse()

    fmt.Printf("port = %d\n", port)
    fmt.Printf("other args: %+v\n", flag.Args())
}
```

中文参考：

* [标准库—命令行参数解析FLAG](http://blog.studygolang.com/2013/02/%E6%A0%87%E5%87%86%E5%BA%93-%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%8F%82%E6%95%B0%E8%A7%A3%E6%9E%90flag/)
* [Go学习笔记：flag库的使用](https://studygolang.com/articles/5608)

更多代码实践：

* [cat demo](https://thenewstack.io/cli-command-line-programming-with-go/)
* [goimports 实现](https://github.com/golang/tools/blob/master/cmd/goimports/goimports.go)

### 4、开发实践

使用 golang 开发 [开发 Linux 命令行实用程序](https://www.ibm.com/developerworks/cn/linux/shell/clutil/index.html) 中的 **selpg**

提示：

1. 请按文档 **使用 selpg** 章节要求测试你的程序
2. 请使用 pflag 替代 goflag 以满足 Unix 命令行规范， 参考：[Golang之使用Flag和Pflag](https://o-my-chenjian.com/2017/09/20/Using-Flag-And-Pflag-With-Golang/)
3. golang 文件读写、读环境变量，请自己查 os 包
4. "-dXXX" 实现，请自己查 `os/exec` 库，例如案例 [Command](https://godoc.org/os/exec#example-Command)，管理子进程的标准输入和输出通常使用  `io.Pipe`，具体案例见 [Pipe](https://godoc.org/io#Pipe) 

### 5、代码提交

* 在 Github 提交程序，并在 readme.md 文件中描述设计说明，使用与测试结果。
* 如果你写了 关于 golang 或其他与课程相关的博客，请在课程群提交记录博客


参考文献：  
[1] Package flag: https://go-zh.org/pkg/flag/  
[2] Package os: https://go-zh.org/pkg/os/  
[3] [CLI: Command Line Programming with Go](https://thenewstack.io/cli-command-line-programming-with-go/)
