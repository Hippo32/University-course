# Vue2.0+Node.js+MongoDB全栈打造商城系统 #
## 前置知识 ##
- HTML/CSS/JS
- Vue基础知识
- ES6
- Node
- Npm
- Webpack

## 课程收获 ##
- 掌握Vue全家桶知识
- 掌握基于Vue的SPA应用开发
- 掌握ES6知识
- 掌握Node+Express的后端接口开发
- 掌握MongoDB等数据库知识
- 掌握Vue+Node项目的线上部署

## 功能 ##
- 商品列表
- 购物车
- 地址
- 结算
- 订单
- 登录模块


----------


视图层

- 商品列表
- 购物车
- 地址列表
- 商品结算
- 订单成功

## Vue环境搭建以及vue-cli使用 ##
Vue多页面应用文件引用

- 官网拷贝：`<script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>`
- npm安装

vue-cli构建SPA应用

- npm install -g vue-cli
- vue init webpack-simple demo
- vue init webpack demo2

项目目录：

- build
	- utils.js：工具的输出、配置 

## 能实现热加载的原因 ##
通过express启动一个服务

## Vue基础语法介绍 ##
事件修饰符

- .stop：阻止冒泡
- .prevent：阻止默认事件

### 引入vue文件 ###
如果是同级目录，通常是加`./`表示当前目录，`../`表示上级目录。

## 路由基础介绍 ##
- 什么是前端路由？
	
	路由是根据不同的url地址展示不同的内容或页面

	前端路由就是把不同路由对应不同的内容或页面的任务交给前端来做，之前是通过服务端根据url的不同返回不同的页面实现的。
- 什么时候使用前端路由？

	在单页面应用，大部分页面结构不变，只改变部分内容的使用
- 前端路由有什么优点和缺点？
	- 优点

		用户体验好，不需要每次都从服务器全部获取，快速展现给用户
	- 缺点

		不利于SEO  
		使用浏览器的前进，后退键的时候会重新发送请求，没有合理地利用缓存  
		单页面无法记住之前滚动的位置，无法在前进，后退的时候记住滚动的位置
- 什么是动态路由？
	- /user/:username
	- /user/:username/post/:post_id
	- mode
		- history
		- hash
- 什么是嵌套路由？
	- 子路由要写绝对路径`<router-link to="/goods/title">1</router-link>`不能写`/title`
- 什么是编程式路由？
	- 通过JS来实现页面的跳转
	- `$router.push("name")`
	- `$router.push({path:"name"})`
	- `$router.push({path:"name?a=123"})`或者`$router.push({path:"name",query:{a:123}})`
	- `$router.go(1)`
- 什么是命名路由和命名视图？
	- 给路由定义不同的名字，根据名字进行匹配
	- 给不同的`router-view`定义名字，通过名字进行对应组件的渲染


## vue-resource ##
使用：

- `<script src="https://cdn.jsdelivr.net/npm/vue-resource@1.5.1"></script>`
- npm install vue-resource 

vue-resource的请求API是按照REST风格设计的，它提供了7种请求API：

- get(url, [options])
- head(url, [options])
- delete(url, [options])
- jsonp(url, [options])
- post(url, [body], [options])
- put(url, [body], [options])
- patch(url, [body], [options])

options对象

|参数|类型|描述|
|---|----|----|
|url|string|请求的URL|
|method|string|请求的HTTP方法，例如：'GET'，'POST'或其他HTTP方法|
|body|Object，FormData string|request body|
|params|Object|请求的URL参数对象|
|headers|Object|request header|
|timeout|number|单位为毫秒的请求超时时间（0表示无超时时间）|
|before|function(request)|请求发送前的处理函数，类似于jQuery的beforeSend函数|
|progress|function(event)|ProgressEvent回调处理函数|
|credentials|boolean|表示跨域请求时是否需要使用凭证|
|emulateHTTP|boolean|发送PUT，PATCH，DELETE请求时以HTTP POST的方式发送，并设置请求头的X-HTTP-Method-Override|
|emulateJSON|boolean|将request body以application/x-www-form-urlencoded content-type发送|

### 全局拦截器interceptors ###
	Vue.http.interceptors.push((request, next) => {
		// ...
		// 请求发送前的处理逻辑
		// ...
		next((response) => {
		// ...
		// 请求发送后的处理逻辑
		// ...
		// 根据请求的状态，response参数会返回给successCallback或errorCallback
		return response
		})
	})