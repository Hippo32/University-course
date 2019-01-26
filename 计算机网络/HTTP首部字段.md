# HTTP首部字段 #
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

## 通用首部字段 ##
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
首部字段Accept-Language用来告知服务器用户代理能够处理的自然语言集（指中文或英文等），以及自然语言集的相对优先级。可一次指定多种自然语言集。

### Authorization ###
	Authorization: Basic dwVub3NlbjpwYXNzd29yZA==
用来告知服务器，用户代理的认证信息（证书值）。通常，想要通过服务器认证的用户代理会在接收到返回的401状态码响应后，把首部字段Authorization加入请求中。共用缓存在接收到含有Authorization首部字段的请求时的操作处理会略有差异。

### Expect ###
	Expect: 100-continue
客户端使用首部字段Expect来告知服务器，期望出现的某种特定行为。因服务器无法理解客户端的期望作出回应而发生错误时，会返回状态码417 Expectation Failed。客户端可以利用该首部字段，写明所期望的扩展。虽然HTTP/1.1规范只定义了100-continue（状态码100 Continue之意）。

### From ###
	From: info@hackr.jp
用来告知服务器使用用户代理的用户的电子邮件地址。通常，其使用目的就是为了显示搜索引擎等用户代理的负责人的电子邮件联系方式。使用代理时，应尽可能包含From首部字段（但可能会因代理不同，将电子邮件地址记录在User-Agent首部字段内）。

### Host ###
虚拟主机运行在同一个IP上，因此使用首部字段Host加以区分

	Host: www.hackr.jp
首部字段Host会告知服务器，请求的资源所处的互联网主机名和端口号。Host首部字段在HTTP/1.1规范内是唯一一个必须被包含在请求内的首部字段。如服务器未设定主机名，那直接发送一个空值即可。

### If-Match ###
形如`If-xxx`这种歌样式的请求首部字段，都可称为条件请求。服务器接收到附带条件的请求后，只有判断指定条件为真时，才会执行请求。

首部字段If-Match，属附带条件之一，它会告知服务器匹配资源所用的实体标记（ETag）值。这时的服务器无法使用弱ETag值。服务器会比对If-Match的字段值和资源的ETag值，仅当两者一致时，才会执行请求。反之，则返回状态码412 Precondition Failed的响应。还可以使用星号指定If-Match的字段值。针对这种情况，服务器将会忽略ETag的值，只要资源存在就处理请求。

### If-Modified-Since ###
如果在If-Modified-Since字段指定的日期时间后，资源发生了更新，服务器会接受请求。

首部字段If-Modified-Since，属附带条件之一，它会告知服务器若If-Modified-Since字段值早于资源的更新时间，则希望能处理该请求。而在指定If-Modified-Since字段值的日期时间之后，如果请求的资源都没有过更新，则返回状态码304Not Modified的响应。If-Modified-Since用于确认代理或客户端拥有的本地资源的有效性。获取资源的更新日期时间，可通过确认首部字段Last-Modified来确定。

### If-None-Match ###
只有在If-None-Match的字段值与ETag值不一致时，可处理该请求。与If-Match首部字段的作用相反。

在GET或HEAD方法中使用首部字段If-None-Match可获取最新的资源。因此，这与使用首部字段If-Modified-Since时有些类似。

### If-Range ###
If-Range字段值若是跟ETag值或更新的日期时间匹配一致，那么就作为范围请求处理。若不一致，则忽略范围请求，返回全部资源。如果不适用If-Range则需要进行两次处理。

### If-Unmodified-Since ###
首部字段If-Unmodified-Since和首部字段If-Modified-Since的作用相反。它的作用的时告知服务器，指定的请求资源只有在字段值内指定的日期时间之后，未发生更新的情况下，才能处理请求。

### Max-Forwards ###
通过TRACE方法或OPTIONS方法，发送包含首部字段Max-Forwards的请求时，该字段以十进制整数形式指定可经过的服务器最大数目。服务器在往下一个服务器转发请求之前，Max-Forwards的值减1后重新赋值。当服务器接收到Max-Forwards值为0的请求时，则不再进行转发，而是直接返回响应。

### Proxy-Authorization ###
接收到从代理服务器发来的认证质询时，客户端会发送包含首部字段Proxy-Authorization的请求，以告知服务器认证所需要的信息。

### Range ###
对于只需获取部分资源的范围请求，包含首部字段Range即可告知服务器资源的指定范围。接收到附带Range首部字段请求的服务器，会在处理请求之后返回状态码为206 Partial Content的响应。无法处理该范围请求时，则会返回状态码200 OK的响应及全部资源。

### Referer ###
首部字段Referer会告知服务器请求的原始资源URI。客户端一般都会发送Referer首部字段给服务器。但当直接在浏览器的地址输入URI，或出于安全性的考虑时，也可以不发送该首部字段。

### TE ###
首部字段TE会告知服务器客户端能够处理响应的传输编码方式及相对优先级。它和首部字段Accept-Encoding的功能很相像，但是用于传输编码。首部字段TE除指定传输编码之外，还可以指定伴随trailer字段的分块传输编码的方式。应用后者时，只需把trailers赋值给该字段值。

### User-Agent ###
首部字段User-Agent会将创建请求的浏览器和用户代理名称等信息传达给服务器。由网络爬虫发起请求时，有可能会在字段内添加爬虫作者的电子邮件地址。此外，如果请求经过代理，那么中间也很可能被添加上代理服务器的名称。

## 响应首部字段 ##
### Accept-Ranges ###
当不能处理范围请求时，`Accept-Ranges:none`  
首部字段Accept-Ranges是用来告知客户端服务器是否能处理范围请求，以指定获取服务器端某个部分的资源。可指定的字段值有两种，可处理范围请求时指定其为bytes，反之则指定其为none。

### Age ###
首部字段Age能告知客户端，源服务器在多久前创建了响应。字段值的单位为秒。若创建该响应的服务器时缓存服务器，Age值是指缓存后的响应再次发起认证到认证完成的时间值。代理创建响应时必须加上首部字段Age。

### ETag ###
首部字段ETag能告知客户端实体标识。它是一种可将资源以字符串形式做唯一性标识的方式。服务器会为每份资源分配对应的ETag值。另外，当资源更新时，ETag值也需要更新。生成ETag值时，并没有统一的算法规则，而仅仅是由服务器来分配。资源被缓存时，就会被分配唯一性标识。若在下载过程中出现连接中断、再连接的情况，都会依照ETag值来指定资源。

- 强ETage值：不论实体发生多么细微的变化都会改变其值。
- 弱ETag值：只用于提示资源是否相同。只有资源发生了根本改变，产生差异时才会改变ETag值。这时，会在字段最开始处附加W/。

### Location ###
使用首部字段Location可以将响应接收方引导至某个与请求URI位置不同的资源。

### Proxy-Authenticate ###
首部字段Proxy-Authenticate会把由代理服务器所要求的认证信息发送给客户端。

### Retry-After ###
首部字段Retry-Afer告知客户端应该在多久之后再次发送请求。主要配合状态码503 Service Unavailable响应，或3xx Redirect响应一起使用。字段值可以指定为具体的日期时间（GMT等格式），也可以是创建响应后的秒数。

### Server ###
首部字段Server告知客户端当前服务器上安装的HTTP服务器应用程序的信息。不单单会标出服务器上的软件应用名称，还有可能包括版本号和安装时启用的可选项。

### Vary ###
当代理服务器接收到带有Vary首部字段指定获取资源的请求时，如果使用的Accept-Language字段的值相同，那么就直接从缓存返回响应。反之，则需要先从源服务器端获取资源后才能作为响应返回。  
首部字段Vary可对缓存进行控制。源服务器会向代理服务器传达关于本地缓存使用方法的命令。从代理服务器接收到源服务器返回包含Vary指定项的响应之后，若再要进行缓存，仅对请求中含有相同Vary指定首部字段的请求返回缓存。即使对相同资源发起请求，但由于Vary指定的首部字段不相同，因此必须要从源服务器重新获取资源。

### WWW-Authenticate ###
首部字段WWW-Authenticate用于HTTP访问认证。它会告知客户端适用于访问URI所指定资源的认证方案（Basic或是Digest）和带参数提示的质询（challenge）。状态码401 Unauthorized响应中，肯定带有首部字段WWW-Authenticate。

## 实体首部字段 ##
### Allow ###
首部字段Allow用于通知客户端能够支持Request-URI指定资源的所有HTTP方法。当服务器接收到不支持的HTTP方法时，会以状态码405 Method Not Allowed作为响应返回。

### Content-Encoding ###
首部字段Content-Encoding会告知客户端服务器对实体的主体部分选用的内容编码方式。内容编码是指在不丢失实体信息的前提下所进行的压缩。

### Content-Language ###
首部字段Content-Language会告知客户端，实体主体使用的自然语言（指中文或英文等语言）。

### Content-Length ###
首部字段Content-Length表明了实体主体部分的大小（单位是字节）。对实体主体进行内容编码传输时，不能再使用Content-Length首部字段。

### Content-Location ###
首部字段Content-Location给出与报文主体部分相对应的URI。和首部字段Location不同，Content-Location表示的是报文主体返回资源对应的URI。

### Content-MD5 ###
客户端会对接收的报文主体执行相同的MD5算法，然后与首部字段Content-MD5的字段值比较。  
首部字段Content-MD5是一串由MD5算法生成的值，其目的在于检查报文主体在传输过程中是否保持完整，以及确认传输到达。

### Content-Range ###
针对范围请求，返回响应时使用的首部字段Content-Range，能告知客户端作为响应返回的实体的哪个部分符合范围请求。字段值以字节为单位，表示当前发送部分及整个实体大小。

### Content-Type ###
首部字段Content-Type说明了实体主体内对象的媒体类型。和首部字段Accept一样，字段值用type/subtype形式赋值。

### Expires ###
首部字段Expires会将资源失效的日期告知客户端。缓存服务器在接收到含有首部字段Expires的响应后，会以缓存来应答请求，在Expires字段值指定的时间之前，响应的副本会一直被保存。当超过指定的时间后，缓存服务器在请求发送过来时，会转向源服务器请求资源。  
但是，当首部字段Cache-Control有指定max-age指令时，比起首部字段Expires，会优先处理max-age指令。

### Last-Modified ###
首部字段Last-Modified指明资源最终修改的时间。一般来说，这个值就是Request-URI指定资源被修改的时间。

## 为Cookie服务的首部字段 ##
|首部字段名|说明|首部类型|
|-|-|-|
|Set-Cookie|开始状态管理所使用的Cookie信息|响应首部字段|
|Cookie|服务器接收到的Cookie信息|请求首部字段|

### Set-Cookie ###
Set-Cookie字段的属性
  
|属性|说明|
|-|-|
|NAME=VALUE|赋予Cookie的名称和其值（必须项）|
|expires=DATE|Cookie的有效期（若不明确指定则默认为浏览器关闭前为止）|
|path=PATH|将服务器上的文件目录作为Cookie的适用对象（若不指定则默认为文档所在的文件目录）|
|domain=域名|作为Cookie适用对象的域名（若不指定则默认为创建Cookie的服务器的域名）|
|Secure|仅在HTTPS安全通信时才会发送Cookie|
|HttpOnly|加以限制，使Cookie不能被JavaScript脚本访问|

### Cookie ###
首部字段Cookie会告知服务器，当客户端想获得HTTP状态管理支持时，就会在请求中包含从服务器接收到的Cookie。接收到多个Cookie时，同样可以以多个Cookie形式发送。

## 其他首部字段 ##
### X-Frame=Options ###
首部字段X-Frame-Options属于HTTP响应首部，用于控制网站内容在其他Web网站的Frame标签内的显示问题。其主要目的是为了防止点击劫持攻击。首部字段X-Frame-Options有一下两个可指定的字段值。

- DENY：拒绝
- SAMEORIGIN：仅同源域名下的页面匹配时许可。

### X-XSS-Protection ###
首部字段X-XSS-Protection属于HTTP响应首部，它是针对跨站脚本攻击（XSS）的一种对策，用于控制浏览器XSS防护机制的开关。首部字段X-XSS-Protection可指定的字段值如下。

- 0：将XSS过滤设置成无效状态
- 1：将XSS过滤设置成有效状态

### DNT ###
首部字段DNT属于HTTP请求首部，其中DNT是Do Not Track的简称，意为拒绝个人信息被收集，是表示拒绝被精准广告追踪的一种方法。首部字段DNT可指定的字段值如下。

- 0：同意被追踪
- 1：拒绝被追踪

### P3P ###
首部字段P3P属于HTTP响应首部，通过利用P3P技术，可以让Web网站上的个人隐私变成一种仅供程序可理解的形式，以达到保护用户隐私的目的。要进行P3P的设定，需按以下操作步骤进行。

1. 创建P3P隐私
2. 创建P3P隐私对照文件后，保存命名在/w3c/p3p.xml
3. 从P3P隐私中新建Compact policies后，输出到HTTP响应中