---
layout: default
title: ssh 安全与自动登陆配置
---

## 服务计算 - ssh 安全与自动登陆配置
{:.no_toc}

许多工具需要使用 ssh 或 scp 等远程工具完成远程主机的自动配置，这些工具通常不会在登陆时完成交互认证过程，因此需要了解一些基于安全密钥的知识，实现自动登陆。

* 目录
{:toc}

## 1、密钥原理

加密技术是高深的，但使用是比较简单的。

* 第一：产生一对不相同数（私钥，公钥），对应消息（M）用其中一个数加密，必须用另一个数解密（破解需要巨大计算量）
* 第二：自己留的一个数就是私钥，给朋友们的就是公钥。这样，你加密的消息 {消息|私钥} 所有朋友都能用公钥阅读，某个朋友加密的 {消息|公钥} 只有你能读
* 第三：签名。掌握私钥的人将 （消息 + {hash（消息）|私钥}） 交给一个朋友。这个朋友就可以宣称拿到你的亲手书，因为文档后面有你的签名（fingerprint）。其他朋友不信啊，它用你给它的公钥解密消息，得到的值等于（hash（消息）），其中hash（消息）相当于消息的指纹。

ssh 就是使用上述原理完成用户认证与信息安全保密的。

## 2、ssh 客户端与服务器的握手

### 2.1 ssh 第一次访问某个远程服务器

ssh 第一次访问某个远程服务器会这样提示：

```
$ ssh root@your.domian.or.ip
The authenticity of host 'your.domian.or.ip (192.168.xxx.xxx)' can't be established.
ECDSA key fingerprint is SHA256:JVic1pDtJ4e62Wz4i+bn8szU0X3dfr9vz8o12dgYisI.
Are you sure you want to continue connecting (yes/no)? yes
```

scp 就会提示 `No ECDSA host key` 

ECDSA就是远程服务器 ssh 给客户端的签名的消息，这个消息存在客户端 `.ssh/known-hosts` 中。

取消这个提示有三种方法：

1. 手工完成第一次访问
2. 在命令行，添加 stricthostkeychecking=no 选项
3. 修改 `/etc/ssh/ssh_config` 配置文件，设置： `StrictHostKeyChecking no`

### 2.2 ssh 第访问某个远程服务器需要用户密码

按以下步骤完成：

**第一步：**，生成 RSA 密钥对

```
$ ssh-keygen
```

按提示完成，在 `.ssh` 目录下生成文件 `id_rsa` 和 `id_rsa.pub`

**第二步**，以后每使用一台服务其，仅需将公钥拷贝到主机对应用户的 `.ssh` 目录中。

例如：

```
$ scp .ssh/id_rsa.pub root@your.domian.or.ip:/root/.ssh/authorized_keys
```

这样实现了一个客户端免密登陆多台远程服务器。








