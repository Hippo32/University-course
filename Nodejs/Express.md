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

参考文档：

- [express4.x官方文档](http://www.expressjs.com.cn/4x/api.html)
- [Express目录结构及各部分功能](https://www.jianshu.com/p/9e06d4e859ab)
- Node.js开发指南
- [express 的router(路由)](https://blog.csdn.net/angle_jian/article/details/78073292)
- [express路由设计](https://blog.csdn.net/zzwwjjdj1/article/details/51839754)

