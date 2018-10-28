# Express #
## 安装 ##
	npm install -g express
我的版本是4.16.0

## 在命令行创建项目时使用ejs模板 ##
	express -e blog
	cd blog
	npm install
	npm start

> 无参数的npm install 的功能就是检查当前目录下的package.json，并自动安装所有指定的依赖。

> npm start 运行的是package.json中script块start对应的命令。

## 工程结构 ##
blog

- bin
- node_modules：存放package.json中安装的模块，当你在package.json添加依赖的模块并安装后，存放在这个文件夹中
- public：存放image、css、js等文件
- routes：存放路由文件
- views：存放视图文件或者说模板文件
- app.js：启动文件，或者说入口文件
- package.json：存储着工程的信息及模块依赖，当在dependencies中添加依赖的模块时，运行`npm install`，npm会检查当前目录下的package.json，并自动安装所有指定的模块
- package-lock.json

### app.js ###
	var createError = require('http-errors');
	var express = require('express');
	var path = require('path');
	var cookieParser = require('cookie-parser');
	var logger = require('morgan');
	
	var indexRouter = require('./routes/index');
	var usersRouter = require('./routes/users');
	
	var app = express();
	
	// 设置views文件夹为存放视图文件的目录，即存放模板文件的地方
	// dirname为全局变量，存储当前正在执行的脚本所在的目录
	
	// view engine setup
	app.set('views', path.join(__dirname, 'views'));
	//  设置模板引擎为ejs
	app.set('view engine', 'ejs');
	
	// 加载日志中间件
	app.use(logger('dev'));
	// 加载解析json的中间件
	app.use(express.json());
	// 加载解析urlencoded请求体的中间件
	app.use(express.urlencoded({ extended: false }));
	// 加载解析cookie的中间件
	app.use(cookieParser());
	// 设置public文件夹为存放静态文件的目录
	app.use(express.static(path.join(__dirname, 'public')));
	
	// 路由器控制器
	app.use('/', indexRouter);
	app.use('/users', usersRouter);
	
	// 捕获404错误，并转发到错误处理器
	// catch 404 and forward to error handler
	app.use(function(req, res, next) {
	  next(createError(404));
	});
	
	// 通用环境下的错误处理器，将错误信息渲染error模板并显示到浏览器中
	// error handler
	app.use(function(err, req, res, next) {
	  // set locals, only providing error in development
	  res.locals.message = err.message;
	  // 开发环境下的错误处理器，将错误信息渲染error模板并显示到浏览器中
	  res.locals.error = req.app.get('env') === 'development' ? err : {};
	
	  // render the error page
	  res.status(err.status || 500);
	  res.render('error');
	});
	
	// 导出app实例，供其他模块调用
	module.exports = app;

### routes/index.js ###
是路由文件，相当于控制器，用于组织展示的内容：

	// 生成一个路由器实例用来捕获访问主页的GET请求，导出整个路由并在app.js中通过app.use('/', routes);加载，
	// 这样，当访问主页时，就会调用res.render('index', { title: 'Express'});渲染views/index.ejs模板并显示到浏览器中
	var express = require('express');
	var router = express.Router();
	
	/* GET home page. */
	router.get('/', function(req, res, next) {
	  res.render('index', { title: 'Express' });
	});
	
	// 导出路由实例
	module.exports = router;

### bin/www ###
	#!/usr/bin/env node

	/**
	 * Module dependencies.
	 */
	
	// 引入app.js导出的app实例
	var app = require('../app');
	// 引入debug模块，打印调试日志
	var debug = require('debug')('blog:server');
	var http = require('http');
	
	/**
	 * Get port from environment and store in Express.
	 */
	// 设置端口号
	var port = normalizePort(process.env.PORT || '3000');
	app.set('port', port);
	
	/**
	 * Create HTTP server.
	 */
	// 启动工程
	var server = http.createServer(app);
	
	/**
	 * Listen on provided port, on all network interfaces.
	 */
	// 监听端口号
	server.listen(port);
	server.on('error', onError);
	server.on('listening', onListening);
	
	/**
	 * Normalize a port into a number, string, or false.
	 */
	
	function normalizePort(val) {
	  var port = parseInt(val, 10);
	
	  if (isNaN(port)) {
	    // named pipe
	    return val;
	  }
	
	  if (port >= 0) {
	    // port number
	    return port;
	  }
	
	  return false;
	}
	
	/**
	 * Event listener for HTTP server "error" event.
	 */
	
	function onError(error) {
	  if (error.syscall !== 'listen') {
	    throw error;
	  }
	
	  var bind = typeof port === 'string'
	    ? 'Pipe ' + port
	    : 'Port ' + port;
	
	  // handle specific listen errors with friendly messages
	  switch (error.code) {
	    case 'EACCES':
	      console.error(bind + ' requires elevated privileges');
	      process.exit(1);
	      break;
	    case 'EADDRINUSE':
	      console.error(bind + ' is already in use');
	      process.exit(1);
	      break;
	    default:
	      throw error;
	  }
	}
	
	/**
	 * Event listener for HTTP server "listening" event.
	 */
	
	function onListening() {
	  var addr = server.address();
	  var bind = typeof addr === 'string'
	    ? 'pipe ' + addr
	    : 'port ' + addr.port;
	  debug('Listening on ' + bind);
	}

`app.set`是Express的参数设置工具，接受一个键（key）和一个值（value），可用的参数如下：

- basepath：基础地址，通常用于res.redirect()跳转。
- views：视图文件的目录，存放模板文件。
- view engine：视图模板引擎。
- view options：全局视图参数对象。
- view cache：视图缓存。
- case sensitive routes：路径区分大小写。
- strict routing：严格路径，启用后不会忽略路径末尾的"/”。
- jsonp callback：开启透明的JSONP支持。

## Express网站架构 ##
![](https://i.imgur.com/wXsywmn.png)

这是一个典型的MVC架构，浏览器发起请求，由路由控制器接受，根据不同的路径定向到不同的控制器。控制器处理用户的具体请求，可能会访问数据库中的对象，即模型部分。控制器还要访问模板引擎，生成视图的HTML，最后由控制器返回给浏览器，完成一次请求。

# Express路由 #
## 创建一个新路由 ##
在app.js中新增

	var regist = require('./routes/regist');
	app.use('/regist', regist);

在views中新增`regist.ejs`

在routes中新增`regist.js`

	var express = require('express');
	var router = express.Router();
	
	// 回馈路由
	router.get('/', function(req, res, next) {
	    res.render('regist', { title: 'Express' });
	});
	
	// 接收post请求
	router.post('/', function(req, res, next) {
	    console.log(req.body.username);
	    res.render('regist', {title: 'john'});
	});
	
	// 导出路由实例
	module.exports = router;

> Express在处理路由规则时，会优先匹配先定义的路由规则，因此后面相同的规则被屏蔽。

## 路由控制权转移 ##
通过调用next()，会将路由控制权转移给后面的规则。这是一个非常有用的工具，可以让我们轻易地实现中间件，而且还能提高代码的复用程度。

# 模板引擎 #
res.render的功能是调用模板引擎，并将其产生的页面直接返回给客户端。它接受两个参数，第一个是模板的名称，即views目录下的模板文件名，不包含文件的扩展名；第二个参数是传递给模板的数据，用于模板翻译。

	res.render('index', { title: 'Express' });

## 会话中间件 ##
默认情况下是把用户信息存储在内存中。


# express模块 #
Node中的核心模块分两类：一类是自带的核心模块，如http、tcp等，第二类是第三方核心模块，express就是与http对应的第三方核心模块，用于处理http请求。express在3.0版本中自带有很多中间件，但是在express4.0以后，就将除static（静态文件处理）以外的其它中间件分离出来了；在4.0以后需要使用中间件时，就需要单独安装好相应的中间件以后调用，以下是3.0与4.0中间件区别（3.0是内置中间件属性名，4.0是需要安装的中间件名称）：

|Express 3.0 Name|Express 4.0 Name|
|-|-|
|bodyParser|body-parser|
|compress|compression|
|cookieSession|cookie-session|
|logger|morgan|
|cookieParser|cookie-parser|
|session|express-session|
|favicon|static-favicon|
|response-time|response-time|
|error-handler|errorhandler|
|method-override|method-override|
|timeout|connect-timeout|
|vhost|vhost|
|csrf|csurf|

## bodyparser ##
方便我们解析浏览器发送来的body数据

四个方法：

- bodyParser.json(options)：处理json数据
- bodyParser.raw(options)：处理Buffer流数据
- bodyParser.text(options):文本数据
- bodyParser.urlencoden(options)：UTF-8的编码的数据

## express-session ##
方便我们处理客户端的session。

由于session的机制离不开cookie，故express-session中创建session时可以接收的options常见的有：cookie，name（，resave（是否每次都重新保存会话）

服务器中生成cookie要存储在客户端时，服务端主要是设置set-cookie头，让它随响应信息response发送到客户端。那么服务器端设置set-cookie头的参数主要通过`response.cookie(name, value[,options])`设置，`response.cookie`是express对象设置cookie的方法。

name：cookie的名字

value：规定cookie的值。

options对象是用来设置set-cookie头部的选项。

|属性|类型|描述|
|-|-|-|
|domain|String|设置cookie的域名。默认是你本app的域名。|
|expires|Date|cookie的过期时间，GMT格式。如果没有指定或者设置为0，则产生新的cookie。|
|httpOnly|Boolean|这个cookie只能被web服务器获取的标示。|
|maxAge|String|是设置过去时间的方便选项，其为过期时间到当前时间的毫秒值。|
|path|String|cookie的路径。默认值是/。|
|srcure|Boolean|表示这个cookie只用被HTTPS协议使用。|
|signed|Boolean|指示这个cookie应该是签名的。|

例：

	app.use(session({
		secret: 'my app secret', // 用来对sessionID相关的cookie进行签名
		saveUninitialized: false, // 是否自动保存未初始化的会话，建议false
		resave: false, // 是否每次都重新保存会话，建议false
		store: new MongoStore({ // 创建新的mongodb数据库存储session
			host: 'localhost', // 数据库的地址，本机的话就是127.0.0.1，也可以是网络主机
			port: 27017, // 数据库的端口号
			db: 'test-app' //数据库的名称
		}),
		name: 'test', // cookie的name，默认值是：connect.sid
		cookies: {
			maxAge: 10*1000
		}
	}));

一旦我们将express-session中间件用use挂载后，我们可以很方便的通过req参数来存储和访问session对象的数据。req.session是一个JSON格式的JavaScript对象，我们可以在使用的过程中随意的增加成员，这些成员会自动的被保存到option参数指定的地方，默认即为内存中去。

## cookie-parser ##
cookie的值value是一串字符或者是转化为json字符串的对象，cookie-parser是将cookie的值由字符串解析成对象的中间件，当使用cookie-parser中间件的时候，可以通过express中的request对象cookies属性`req.cookies`查看cookies的值，比如`req.cookies.name`。`req.cookies`这个属性是一个对象，其包含了请求发送过来的cookies。如果请求没有带cookies，那么其值为{}。

## config-lite ##
config-lite是一个轻量的读取配置文件的模块。

config-lite会根据环境变量（NODE-ENV）的不同从当前执行进程目录下的config目录加载不同的配置文件。

参考文档：

- [express4.x官方文档](http://www.expressjs.com.cn/4x/api.html)
- [Express目录结构及各部分功能](https://www.jianshu.com/p/9e06d4e859ab)
- Node.js开发指南
- [express 的router(路由)](https://blog.csdn.net/angle_jian/article/details/78073292)
- [express路由设计](https://blog.csdn.net/zzwwjjdj1/article/details/51839754)

