# Node.js的HTTP模块 #
## HTTP服务器 ##
`http.Server`是一个基于事件的HTTP服务器，它的核心由Node.js下层C++部分实现，而接口由JavaScript封装，兼顾了高性能与简易型。用Node.js做的所有基于HTTP协议的系统，如网站、社交应用甚至代理服务器，都是基于`http.Server`实现的。它提供了一套封装级别很低的API，仅仅是流控制和简单的消息解析，所有的高层功能都要通过它的接口来实现。

### http.Server的事件 ###
`http.Server`是一个基于事件的HTTP服务器，所有的请求都被封装为独立的事件，开发者只需要对它的事件编写响应函数即可实现HTTP服务器的所有功能。

- `request`：当客户端请求到来时，该事件被触发，提供两个参数`req`和`res`，分别是`http.ServerRequest`和`http.ServerResponse`的实例，表示请求和响应信息。
- `connection`：当TCP连接建立时，该事件被触发，提供一个参数`socket`，为`net.Socket`的实例。
- `close`：当服务器关闭时，该事件被触发。注意不是在用户连接断开时。

### http.createServer([requestListener]) ###
功能是创建一个HTTP服务器并将`requestListener`作为`request`事件的监听函数。这个是监听request事件的快捷方式。

### http.ServerRequest ###
`http.ServerRequest`是HTTP请求的信息。它一般由`http.Server`的`request`事件发送，作为第一个参数传递。

用于控制请求体传输的3个事件：

- data：当请求体数据到来时，该事件被触发。该事件提供一个参数chunk，表示接收到的数据。如果该事件没有被监听，那么请求体将会被抛弃。该事件可能会被调用多次。
- end：当请求体数据传输完成时，该事件被触发，此后将不会再有数据到来。
- close：用户当前请求结束时，该事件被触发。不同于end，如果用户强制终止了传输，也还是调用close。

> 获取GET请求的参数（GET请求直接嵌入在路径中）：`url.parse(req.url, true)`

> 获取POST请求的内容（POST请求的内容全部都在请求体中），没有可以直接获取请求体的方法。

### http.ServerResponse ###
`http.ServerResponse`是返回客户端的信息，决定了用户最终能看到的结果。它也是由`http.Server`的`request`事件发送的，作为第二个参数传递。

三个重要的成员函数，用于返回响应头、响应内容以及结束请求。

- `response.writeHead(statusCode, [headers])`：向请求的客户端发送相应头。`statusCode`是HTTP状态码。`headers`是一个类似关联数组的对象，表示响应头的每个属性，该函数在一个请求内最多只能调用一次，如果不调用，则会自动生成一个响应头。
- `response.write[data, [encoding])`：向请求的客户端发送响应内容。data是一个Buffer或字符串，表示要发送的内容。如果data是字符串，那么需要指定encoding来说明它的编码方式，默认是utf-8。
- `response.end([data],[encoding])`：结束响应，告知客户端所有发送已经完成。当所有要返回的内容发送完毕的时候，该函数必须被调用一次。

例子：
	
	var http = require('http');
	var url = require('url');
	var util = require('util');
	
	http.createServer(function(req, res) {
		res.writeHead(200, {'Content-Type': 'text/plain'});
		res.end(util.inspect{url.parse(req.url, true)});
	}).listen(3000);
## HTTP客户端 ##
http模块提供了两个函数`http.request`和`http.get`，功能是作为客户端向HTTP服务器发起请求。

### http.request(options, callback) ###
发起HTTP请求。

参数：

- options：一个类似关联数组的对象，表示请求的参数。
	- host：请求网站的域名或IP地址。
	- port：请求网站的接口，默认是80.
	- method：请求方法，默认是GET。
	- path：请求的相对于根的路径，默认是`"/"`。QueryString应该包含在其中。例如`/search?query=byvoid`。
	- headers：一个关联数组对象，为请求头的内容。
- callback：请求的回调函数
	- 传递一个参数，为`http.ClientResponse`的实例

返回值：一个`http.ClientRequest`的实例。

不要忘了通过`req.end()`结束请求，否则服务器将不会收到信息。

### http.get(options, callback) ###
处理GET请求。是`http.request`的简化版。唯一的区别在于`http.get`自动将请求方法设为GET请求，同时不需要手动调用`req.end()`。

### http.ClientRequest ###
`http.ClientRequest`是由`http.request`或`http.get`返回产生的对象，表示一个已经产生而且正在进行中的HTTP请求。它提供一个`response`事件，即`http.request`或`http.get`第二个参数指定的回调函数的绑定对象。

函数：

- write()和end()函数：用于向服务器发送请求体，通常用于POST、PUT等操作。所有写结束以后必须调用end函数以通知服务器，否则请求无效。
- request.abort()：终止正在发送的请求。
- request.setTimeout(timeout, [callback])：设置请求超时时间，timeout为毫秒数。当请求超时以后，callback将会被调用。

### http.ClientResponse ###
与`http.ServerRequest`相似，提供了三个事件data、end和close，分别在数据到达、传输结束和连接结束时触发，其中data事件传递一个参数chunk，表示接收到的数据。

函数：

- response.setEncoding([encoding])：设置默认的编码，当data事件被触发时，数据将会以encoding编码。默认是null，即不编码，以Buffer的形式存储。常用编码为utf8.
- response.pause()：暂停接收数据和发送事件，方便实现下载功能。
- response.resume()：从暂停的状态中恢复。