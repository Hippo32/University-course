# HTTP #
HTTP协议解决文件传输的问题。HTTP是应用层协议。
HTTP是基于传输层TCP协议的，而TCP是一个端到端的面向连接的协议。所谓的端到端可以理解为进程到进程之间的通信。所以HTTP在开始传输之前，首先需要建立TCP连接，而TCP连接的过程需要所谓的“三次握手”。

HTTP是一种不保存状态，即无状态协议。HTTP协议自身不对请求和响应之间的通信状态进行保存。这是为了更快地处理大量事务，确保协议的可伸缩性，而特意把HTTP协议设计成如此简单的。

> HTTP/1.1虽然是无状态协议，但为了实现期望的保持状态功能，于是引入了Cookie技术。有了Cookie再用HTTP协议通信，就可以管理状态了。

## 简单三次握手 ##
![](https://i.imgur.com/E18Hj9k.png)

![](https://i.imgur.com/3m5vEGl.png)

![](https://i.imgur.com/H08B9wh.png)
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
方法、资源的路径、协议的版本、可选的请求首部字段和内容实体构成。

请求首部字段：

	Host: hackr.jp
	Connection: keep-alive
	Content-Type: application/x-www-form-urlencoded
	Content-Length: 16

内容实体：

	name=ueno&age=37

## HTTP请求方法 ##
- GET：从指定的资源请求数据
- POST：向指定的资源提交要被处理的数据
- PUT：传输文件

	就像FTP协议的文件上传一样，要求在请求报文的主体中包含文件内容，然后保存到请求URI指定的位置。
- DELETE：删除文件

	是与PUT相反的方法。DELETE方法按请求URI删除指定的资源。
- HEAD：请求响应报文首部

	和GET一样，但不返回报文主体。用于确认URI的有效性及资源更新的日期时间等。
- OPTIONS：返回请求的资源所支持的方法的方法
- CONNECT：要求用隧道协议连接代理

	CONNECT方法要求在与代理服务器通信时建立隧道，实现用隧道协议进行TCP通信。主要使用SSL和TLS协议把通信内容加密后经网络隧道传输。

	格式：
		
		CONNECT 代理服务器名:端口号 HTTP版本
- TRACE：追踪路径

	TRACE方法是让Web服务器端将之前通信环回给客户端的方法。

	发送请求时，在Max-Forwards首部字段中填入数值，每经过一个服务器端就将该数字减1，当数值刚好减到0时，就停止继续传输，最后接收到请求的服务端则返回状态码200 OK的响应。

	客户端通过TRACE方法可以查询发送出去的请求是怎样被加工修改/篡改的。这是因为，请求想要连接到源目标服务器可能会通过代理中转，TRACE方法就是用来确认连接过程中发生的一系列操作。

	TRACE方法容易引发XST（Cross-Site Tracing，跨站追踪）攻击。


方法名区分大小写，注意要用大写字母。

![](https://i.imgur.com/65JlR5I.png)
### 比较GET与POST ###
GET方法：

- 查询字符串（名称/值对）是在GET请求的URL中发送的。
- GET请求可被缓存
- GET请求保留在浏览器历史记录中
- GET请求可被收藏为书签
- GET请求不应在处理敏感数据时使用
- GET请求只应当于取回数据

POST方法：

- 查询字符串（名称/值对）是在POST请求的HTTP消息主体中发送的。
- POST请求不会被缓存
- POST请求不会保留在浏览器历史记录中
- POST不能被收藏为书签
- POST请求对数据长度没有要求

![](https://i.imgur.com/t2MQWbq.png)

参考文章：

- [GET和POST两种基本请求方法的区别](https://www.cnblogs.com/logsharing/p/8448446.html)

## 指定请求URI ##
### 方法一 URI为完整的请求URI ###
	GET http://hackr.jp/index.htm HTTP/1.1
### 方法二 在首部字段Host中写明网络域名或IP地址 ###
	GET /index.htm HTTP/1.1
	Host: hackr.jp
### 其他 ###
除此之外，如果不是访问特定资源而是对服务器本身发起请求，可以用一个`*`来代替请求URI。

例子：查询HTTP服务器端支持的HTTP方法种类

	OPTIONS * HTTP/1.1
## HTTP响应 ##
1. 状态行
2. HTTP头
	- 请响应头（response header）
	- 普通头（general header）
	- 实体头（entity header）
3. 返回内容

一个典型的HTTP状态如下：

	HTTP/1.1 200 OK
第一部分是HTTP版本，第二部分是响应状态码，第三部分是状态码的描述（原因短语）。

	HTTP/1.1 200 OK
	Date: Tue, 10 Jul 2012 06:50:15 GMT	
	Content-Length: 362
	Content-Type: text/html

	<html>
	...

显示了创建响应的日期时间，是首部字段内的一个属性。接着以一空行分隔，之后的内容称为资源实体的主体

> 响应报文基本上由协议版本、状态码（表示请求成功或失败的数字代码）、用以解释状态码的原因短语、可选的响应首部字段以及实体主体构成。

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

## 持久连接 ##
持久连接的特点是，只要任意一端没有明确提出断开连接，则保持TCP连接状态。持久连接旨在建立1次TCP连接后进行多次请求和响应的交互。

持久连接的好处在于减少了TCP连接的重复建立和断开所造成的额外开销，减轻了服务器端的负载。

在HTTP/1.1中，所有的连接默认都是持久连接，但在HTTP/1.0内并未标准化。

## 管线化 ##
不用等待响应亦可直接发送下一个请求。这样就能够做到同时并行发送多个请求，而不需要一个接一个地等待响应了。

## HTTP报文 ##
用于HTTP协议交互的信息被称为HTTP报文。HTTP报文本身是由多行（用CR+LF作换行符）数据构成的字符串文本。

HTTP报文大致可分为报文首部和报文主体两块。两者由最初出现的空行（CR+LF）来划分。通常，并不一定要有报文主体。

报文首部：服务器端或客户端需处理的请求或响应的内容及属性。

![](https://i.imgur.com/y2qliqT.png)

![](https://i.imgur.com/1DDjQF0.png)

- 请求行：包含用于请求的方法，请求URI和HTTP版本。
- 状态行：包含表明响应结果的状态码，原因短语和HTTP版本。
- 首部字段：包含表示请求和响应的各种条件和属性的各类首部。
	- 通用首部
	- 请求首部
	- 响应首部
	- 实体首部
- 其他：可能包含HTTP的RFC里未定义的首部（Cookie等）。

## 报文主体和实体主体 ##
- 报文：是网络中交换和传输的数据单元。即站点一次性要发送的数据块。报文包含了将要发送的完整的数据信息。
- 实体：作为请求或响应的有效载荷数据（补充项）被传输，其内容由实体主体组成。

![](https://i.imgur.com/vwVatz5.png)

我们可以看到，上面示例右图中深红色框的内容就是报文的实体部分，而蓝色框的两部分内容就是实体首部和实体主体。而左图中粉红色框内容就是报文主体。通常，**报文主体等于实体主体。只有当传输中进行编码操作时，实体主体的内容发生变化，才导致它和报文主体产生差异。**

## 内容编码 ##
内容编码指明应用在实体内容上的编码格式，并保持实体信息原样压缩。内容编码后的实体由客户端接收并负责解码。

常用的内容编码有以下几种。

- gzip（GNU zip）
- compress（UNIX系统的标准压缩）
- deflate（zlib）
- identity（不进行编码）

## 分块传输编码 ##
分块传输编码会将实体主体分成多个部分（块）。每一块都会用十六进制来标记块的大小，而实体主体的最后一块会使用“0(CR+LF)”来标记。

## 多部分对象集合 ##
MIME：多用途因特网邮件扩展。允许邮件处理文本、图片、视频等多个不同类型的数据。MIME扩展中会使用一种称为多部分对象集合（Multipart）的方法，来容纳多份不同类型的数据。

- multipart/form-data
- multipart/byteranges

在HTTP报文中使用多部分对象集合时，需要在首部字段里加上`Content-type`。使用`boundary`字符串来划分多部分对象集合指明的各类实体。在`boundary`字符串指定的各个实体的起始行之前插入`--`标记（例如：`--AaB03x`、`--THIS_STRING_SEPARATES`），而在多部分对象集合对应的字符串的最后插入`--`标记。

## 内容协商 ##
内容协商机制是指客户端和服务器端就响应的资源内容进行交涉，然后提供给客户端最为适合的资源。内容协商会以响应资源的语言、字符集、编码方式等作为判断的基准。   
包含在请求报文中的某些首部字段就是判断的基准。

- Accept
- Accept-Charset
- Accept-Encoding
- Accept-Language
- Content-Language

内容协商技术有以下3种类型。

- 服务器驱动协商
- 客户端驱动协商
- 透明协商

## HTTP首部字段 ##
HTTP首部字段是构成HTTP报文的要素之一。。使用首部字段是为了给浏览器给服务器提供报文主体大小、所使用的语言、认证信息等内容。

HTTP首部字段是由首部字段名和字段值构成的，中间用冒号`:`分隔。

HTTP首部字段根据实际用途被分为以下4种类型。

- 通用首部字段：请求报文和响应报文两方都会使用的首部。
- 请求首部字段：从客户端向服务器端发送请求报文时使用的首部。补充了请求的附加内容、客户端信息、响应内容相关优先级等信息。
- 响应首部字段：从服务器端向客户端返回响应报文时使用的首部。补充了响应的附加内容，也会要求客户端附加额外的内容信息。
- 实体首部字段：针对请求报文和响应报文的实体部分使用的首部。补充了资源内容更新时间等与实体有关的信息。

![](https://i.imgur.com/rnKmAze.png)

![](https://i.imgur.com/IMpV5xB.png)

![](https://i.imgur.com/eqzILfF.png)

![](https://i.imgur.com/3ZkgkJF.png)

![](https://i.imgur.com/Ee3wNeZ.png)

### Cache-Contorl ###
	Cache-Control:public
当指定使用public指令时，则明确表明其他用户也可利用缓存。
	
	Cache-Control:private
当指定使用private指令后，响应只以特定的用户作为对象。
	
	Cache-Control:no-cache
使用no-cache指令的目的是为了防止从缓存中返回过期的资源。客户端发送的请求中如果包含`no-cache`指令，则表示客户端将不会接收缓存过的响应。于是，“中间”的缓存服务器必须把客户端请求转发给源服务器。如果服务器返回的响应中包含`no-cache`指令，那么缓存服务器不能对资源进行缓存。源服务器以后也将不再对缓存服务器请求中提出的资源有效性进行确认，且禁止其对响应资源进行缓存操作。

	Cache-Control:no-cache=Location
由服务器返回的响应中，若报文首部字段`Cache-Control`中对`no-cache`字段名具体指定参数值，那么客户端在接收到这个被指定参数值的首部字段对应的响应报文后，就不能使用缓存。换言之，无参数值的首部字段可以使用缓存。只能在响应指令中指定该参数。

	Cache-Control:no-store
当使用`no-store`指令时，暗示请求（和对应的响应）或响应中包含机密信息。该指令规定缓存不能在本地存储请求或响应的任一部分。
	
	Cache-Control: s-maxage=604800(单位：秒)
`s-maxage`指令的功能和`max-age`指令的相同，它们的不同点是`s-maxage`指令只适用于供多位用户使用的公共缓存服务器。当使用`s-maxage`指令后，则直接忽略对`Expires`首部字段及`max-age`指令的处理。

	Cache-Control: max-age=604800（单位：秒）
当客户端发送的请求中包含`max-age`指令时，如果判定缓存资源的缓存时间数值比指定时间的数值更小，那么客户端就接收缓存的资源。另外，当指定`max-age`值为0，那么缓存服务器通常需要将请求转发给源服务器。应用HTTP/1.1版本的缓存服务器遇到同时存在Expires首部字段的情况时，会优先处理`max-age`指令，而忽略掉`Expires`首部字段。而HTTP/1.0版本的缓存服务器的情况却相反，`max-age`指令会被忽略掉。

	Cache-Control: min-fresh=60(单位：秒)
`min-fresh`指令要求缓存服务器返回至少还未过指定时间的缓存资源。

	Cache-Control: max-stale=3600（单位：秒）
使用`max-stale`可指示缓存资源，即使过期也照常接收。如果指令未指定参数值，那么无论经过多久，客户端都会接收响应；如果指令中指定了具体数值，那么即使过期，只要仍处于`max-stale`指定的时间内，仍旧会被客户端接收。

	Cache-Control: only-if-cached
使用`only-if-cached`指令表示客户端仅在缓存服务器本地缓存目标资源的情况下才会要求其返回。

	Cache-Control: must-revalidate
使用`must-revalidate`指令，代理会向源服务器再次验证即将返回的响应缓存目前是否仍然有效。另外，使用`must-revalidate`指令会忽略请求的`max-stale`指令（即使已经在首部使用了`max-stale`，也不会再有效果）。

	Cache-Control: proxy-revalidate
`proxy-revalidate`指令要求所有的缓存服务器在接收到客户端带有该指令的请求返回响应之前，必须再次验证缓存的有效性。

	Cache-Control: no-transform
使用`no-transform`指令规定无论是在请求还是响应中，缓存都不能改变实体主体的媒体类型。

### Connection ###
Connection首部字段具备如下两个作用。

- 控制不再转发给代理的首部字段
- 管理持久连接

		Connection: 不再转发的首部字段名
		Connection: close
HTTP/1.1版本的默认连接都是持久连接。为此，客户端会在持久连接上连续发送请求。当服务器端想明确断开连接时，则指定Connection首部字段的值为Close。
	
		Connection: Keep-Alive
HTTP/1.1之前的HTTP版本的默认连接都是非持久连接。

### Date ###
首部字段`Date`表明创建HTTP报文的日期和时间。

HTTP/1.1协议使用在RFC1123中规定的日期时间的格式，如下示例。

	Date: Tue, 03 Jul 2012 04:40:59 GMT
之前的HTTP协议版本周使用在RFC850中定义的格式，如下所示。

	Date: Tue 03-Jul-12 04:40:59 GMT

### Pragma ###
Pragma时HTTP/1.1之前版本的历史遗留字段，仅作为与HTTP/1.0的向后兼容而定义。

	Pragma: no-cache
该首部字段属于通用首部字段，但用在客户端发送的请求中。客户端会要求所有的中间服务器不返回缓存的资源。

### Trailer ###
首部字段`Trailer`会事先说明在报文主体后记录了哪些首部字段。该首部字段可应用在HTTP/1.1版本分块传输编码时。

### Transfer-Encoding ###
首部字段`Transfer-Encoding`规定了传输报文主体时采用的编码方式。

### Upgrade ###
首部字段`Upgrade`用于检测HTTP协议及其他协议是否可使用更高的版本进行通信，其参数值可以用来指定一个完全不同的通信协议。`Upgrade`首部字段产生作用的Upgrade对象仅限于客户端和邻接服务器之间。因此，使用首部字段Upgrade时，还需要额外指定Connection:Upgrade。对于附有首部字段Upgrade的请求，服务器可用101 Switching Protocols状态码作为响应返回。

### Via ###
使用首部字段Via时为了追踪客户端与服务器之间的请求和响应报文的传输路径。  
报文经过代理或网关时，会先在首部字段Via中附加该服务器的信息，然后再进行转发。  
首部字段Via不仅用于追踪报文的转发，还可避免请求回环的发生。所以必须在经过代理时附加该首部字段内容。  
Via首部是为了追踪传输路径，所以经常会和TRACE方法一起使用。

### Warning ###
HTTP/1.1的Warning首部是从HTTP/1.0的响应首部（Retry-After）演变过来的。该首部通常会告知用户一些与缓存相关的问题的警告。

	Warning: [警告码][警告的主机:端口号] "[警告内容]" ([日期时间])
HTTP/1.1中定义了7中警告。警告码对应的警告内容仅推荐参考。另外，警告码具备扩展性。

HTTP/1.1 警告码

|警告码|警告内容|说明|
|:-:|-|-|
|110|Response is stale（响应已过期）|代理返回已过期的资源|
|111|Revalidation failed（再验证失败）|代理再验证资源有效性时失败（服务器无法到达等原因）|
|112|Disconnection Operation（断开连接操作）|代理与互联网连接被故意切断|
|113|Heuristic expiration（试探性过期）|响应的使用期超过24小时（有效缓存的设定时间大于24小时的情况下）|
|199|Miscellaneous warning（杂项警告）|任意的警告内容|
|214|Transformation applied（使用了转换）|代理对内容编码或媒体类型等执行了某些处理时|
|299|Miscellaneous persistent warning（持久杂项警告）|任意的警告内容|

## 请求首部字段 ##
### Accept ###
	Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept首部字段可通知服务器，用户代理能够处理的媒体类型及媒体类型的相关优先级。可使用`type/subtype`这种形式，一次指定多种媒体类型。  
若想要给现实的媒体类型增加优先级，则使用`q=`来额外表示权重值，用分号（`;`）进行分隔。权重值q的范围是0~1（可精确到小数点后3位），且1为最大值。不指定权重q值时，默认权重为q=1.0。当服务器提供多种内容时，将会首先返回权重值最高的媒体类型。

### Accept-Charset ###
	Accept-Charset: iso-8859-5,unicode-1-1;q=0.8
Accept-Charset首部字段可用来通知服务器用户代理支持的字符集及字符集的相对优先顺序。另外，可一次性指定多种字符集。

### Accept-Encoding ###
	Accept-Encoding: gzip, deflate
Accept-Encoding首部字段用来告知服务器用户代理支持的内容编码及内容编码的优先级顺序。可一次性指定多种内容编码。

- gzip：由文件压缩程序gzip（GNU zip）生成的编码格式（RFC1952），采用Lempel-Ziv算法（LZ77）及32位循环冗余校验（CRC）。
- compress：由UNIX文件压缩程序compress生成的编码格式，采用Lempel-Ziv-Welch算法（LZW）。
- deflate：组合使用zlib（RFC1950）及由deflate压缩算法（RFC1951）生成的编码格式。
- identify：不执行压缩或不会变化的默认编码格式

### Accept-Language ###
	Accept-Language: zh-cn,zh;q=0.7,en-us,en;q=0.3
首部字段Accept-Language用来告知服务器用户代理能够处理的自然语言集（指中文或英文等），以及自然语言集的相对优先级。可一次指定多种