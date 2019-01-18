# -Web
### 一、概述
设计一个 web 小应用，展示静态文件服务、js 请求支持、模板输出、表单处理、Filter 中间件设计等方面的能力。（不需要数据库支持）

### 二、任务
编程 web 应用程序 cloudgo-io。 请在项目 README.MD 给出完成任务的证据！

基本要求

    支持静态文件服务
    支持简单 js 访问
    提交表单，并输出一个表格
    对 /unknown 给出开发中的提示，返回码 5xx


### 三、实现基本功能

#### 1、支持静态文件服务
访问静态文件目录

[点击并拖拽以移动]


查看css/index.css

[点击并拖拽以移动]

查看images/1.jpg

[点击并拖拽以移动]
#### 2、支持简单 js 访问

访问/getInfo路径，获取并显示文本信息

[点击并拖拽以移动]
#### 3、提交表单，并输出一个表格

输入表单/输出表格

[点击并拖拽以移动]



#### 4、对 /unknown 给出开发中的提示，返回码 5xx

访问localhost:8080/unknown路径，返回501错误。

### 四、提高要求

#### 1、分析阅读`gzip`过滤器的源码
##### gzip简介
GZIP最早由Jean-loup Gailly和Mark Adler创建，用于UNⅨ系统的文件压缩。我们在Linux中经常会用到后缀为.gz的文件，它们就是GZIP格式的。现今已经成为Internet 上使用非常普遍的一种数据压缩格式，或者说一种文件格式。
HTTP协议上的GZIP编码是一种用来改进WEB应用程序性能的技术。大流量的WEB站点常常使用GZIP压缩技术来让用户感受更快的速度。这一般是指WWW服务器中安装的一个功能，当有人来访问这个服务器中的网站时，服务器中的这个功能就将网页内容压缩后传输到来访的电脑浏览器中显示出来。一般对纯文本内容可压缩到原大小的40%.这样传输就快了，效果就是你点击网址后会很快的显示出来。当然这也会增加服务器的负载. 一般服务器中都安装有这个功能模块的。

#### 2、编写中间件，使得用户可以使用 gb2312 或 gbk 字符编码的浏览器提交表单、显示网页。
这部分我调用了`github.com/axgle/mahonia`库中的函数。这部分的字符格式转换函数已写好，但是由于无法获取请求报文头部的关于字符编码格式的值，而且没有相应的测试环境，因此最终没有实现。
```go
// 位于converter.go
package service

import (
    "github.com/axgle/mahonia"
)
// 字符格式转换函数
func ConvertToString(src, srcCode, tarCode string) string {
    srcCoder := mahonia.NewDecoder(srcCode)
    srcResult := srcCoder.ConvertString(src)
    tarCoder := mahonia.NewDecoder(tarCode)
    _, cdata, _ := tarCoder.Translate([]byte(srcResult), true)
    result := string(cdata)
    return result
}
```
