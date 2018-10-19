# HTTP #
HTTP协议解决文件传输的问题。HTTP是应用层协议。
HTTP是基于传输层TCP协议的，而TCP是一个端到端的面向连接的协议。所谓的端到端可以理解为进程到进程之间的通信。所以HTTP在开始传输之前，首先需要建立TCP连接，而TCP连接的过程需要所谓的“三次握手”

## 简单三次握手 ##
![](https://i.imgur.com/E18Hj9k.png)

![](https://i.imgur.com/3m5vEGl.png)
## HTTP请求 ##
所谓的HTTP请求，也就是Web客户端向Web服务器发送信息，这个信息由如下三部分组成：

1. 请求行：只有一行
2. HTTP头：可以有多行，每一行是一对键值对。 
	- 请求头（request header）
	- 普通头（general header）
	- 实体头（entity header）
3. 内容（只在POST请求中存在）

一个典型的请求行比如：

	GET www.baidu.com HTTP/1.1
方法、资源的路径、协议的版本

## HTTP请求方法 ##
- GET
- POST
- PUT
- DELETE
- HEAD：仅请求响应首部
- OPTIONS：返回请求的资源所支持的方法的方法
- CONNECT
- TRACE

## HTTP响应 ##
1. 状态行
2. HTTP头
	- 请响应头（response header）
	- 普通头（general header）
	- 实体头（entity header）
3. 返回内容

一个典型的HTTP状态如下：

	HTTP/1.1 200 OK
第一部分是HTTP版本，第二部分是响应状态码，第三部分是状态码的描述。

## HTTP状态码分类 ##
- 信息类（100-199）
- 响应成功（200-299）
- 重定向类（300-399）
- 客户端错误类（400-499）
- 服务端错误类（500-599）


----------
当我们在浏览器的地址栏输入www.linux178.com，然后回车，回车这一瞬间到看到页面到底发生了什么呢？

域名解析 --> 发去TCP的3次握手 --> 建立TCP连接后发起http请求 --> 服务器响应http请求，浏览器得到html代码 --> 浏览器解析html代码，并请求html代码中的资源（如js、css、图片等） --> 浏览器对页面进行渲染呈现给用户

### 域名解析 ###
1. 搜索浏览器自身的DNS缓存[chrome://net-internals/#dns](chrome://net-internals/#dns)
2. 操作系统自身的DNS缓存 `ipconfig /displaydns`
3. 尝试读取hosts文件（位于C:\windows\System32\drivers\etc)
4. 向本地配置的首选DNS服务器发起域名解析请求。。。

参考文章：

- [一次完整的HTTP事务是怎样一个过程？](http://blog.51cto.com/linux5588/1351007)
