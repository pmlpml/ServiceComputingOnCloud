## 处理 web 程序的输入与输出

### 1、概述

设计一个 web 小应用，展示静态文件服务、js 请求支持、模板输出、表单处理、Filter 中间件设计等方面的能力。（不需要数据库支持）

### 2、任务要求

编程 web 应用程序 cloudgo-io。 请在项目 README.MD 给出完成任务的证据！

**基本要求**

1. 支持静态文件服务
2. 支持简单 js 访问
3. 提交表单，并输出一个表格
4. 对 `/unknown` 给出开发中的提示，返回码 `5xx`

**提高要求**

以下任务可以选择完成：

1. 分析阅读 gzip 过滤器的源码（就一个文件 126 行） 
2. 编写中间件，使得用户可以使用 `gb2312` 或 `gbk` 字符编码的浏览器提交表单、显示网页。（服务器模板统一用 utf-8）

提示：

需要关注的 headers 与 格式

REQUEST：
Accept-Charset: gb2312,utf-8;q=0.7,*;q=0.7
Content-type: application/x-www-form-urlencoded;charset:gb2312 (仅 post)

RESPONSE：
Content-Type: text/html;charset=gb2312