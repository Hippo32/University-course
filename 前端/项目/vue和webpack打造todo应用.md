# Vue+Webpack打造todo应用 #
学习时间：两天

学习地址：[https://www.imooc.com/learn/935](https://www.imooc.com/learn/935)

2018/10/31 9:19:08 

## 搭建项目 ##
新建一个项目文件夹，然后

	npm init
生成一个package.json文件。

	npm i webpack vue vue-loader
	npm i css-loader vue-template-compiler
新建src目录，作为源码放置目录。在里面新建`app.vue`。

在根目录新建一个`webpack.config.js`文件。

需要安装的项目以依赖项

- webpack
- vue
- vue-loader：是一个webpack的loader，它允许你以单文件.vue的格式撰写Vue组件。`vue-loader`会解析文件，提取每个语言块，如有必要会通过其他loader处理，最后将他们组装成一个CommonJS模块，`module.exports`出一个Vue.js组件对象。

	特性：

	1. ES2015默认支持
	2. 允许对VUE的组件部分使用其他webpack loader
	3. .vue文件中允许自定义节点，然后使用自定义的loader处理他们
	4. 对`<style><template>`中的静态资源当做模块来对待，并且使用webpack loaders进行处理
	5. 对每个组件模拟出CSS作用域
	6. 支持开发期组件的热重载

	例子：


		// webpack.config.js
		const VueLoaderPlugin = require('vue-loader/lib/plugin')

		module: {
			rules: [
				// 其他规则
				{
        			test: /\.vue$/,
            		loader: 'vue-loader'
        		}
			]
		},
		plugins: [
			// 请确保引入这个插件！
			new VueLoaderPlugin()
		]
- autoprefixer：自动补全css前缀，使用can i use的数据来决定哪些前缀是需要的

	In webpack you can use postcss-loader with autoprefixer and other PostCss plugins.

		module.exports = {
			module: {
				rules: [
					{
						test: /\.css$/,
						use: ["style-loader", "css-loader", "postcss-loader"]
					}
				]
			}
		}
	And create a postcss.config.js with:
	
		const autoprefixer = require('autoprefixer')
		module.exports = {
			plugins: [
				require('autoprefixer')
			]
		}
- css-loader：解释`@import()`和`url()`，会`import/require()`后再解析（resolve）它们。
- babel-loader：作为webpack的loader的一种，作用同其他loader一样，实现对特定文件类型的处理。虽然webpack本身就能够处理`.js`文件，但无法对ES2015+的语法进行转换，babel-loader的作用正是对使用了ES2015+语法的`.js`文件进行处理。

		npm install -D babel-loader
	注意：babel-loader 8.x对应的是@babel/core @babel/preset-env  
		 babel-loader 7.x对应的是babel-core babel-preset-env
- babel-core：作用在于提供一系列api。这便是说，当webpack使用babel-loader处理文件时，babel-loader实际上调用了babel-core的api
- babel-preset-env：作用是告诉babel使用哪种转码规则进行文件处理。

## 构建生产环境 ##
生产环境打包，目的只有两个：

1. 压缩应用代码；
2. 去除Vue.js中的警告

下面是例子：

	// webpack.config.js
	const path = require('path') // path是node中的一个模块，用来处理文件的路径
	module.exports = {
		plugins: [
			new webpack.DefinePlugin({
				'process.env': {
					NODE_ENV: '"production"'
				}
			}),
		]
	}
`DefinePlugin`可以在编译时期创建全局变量。

> 注意，该plugin直接做文本替换，指定的值必须包括引号。

`DefinePlugin`中的每个key都是一个定义。

- 如果value是string，该value会被用作代码块
- 如果value不是string，该value是字符串化的（包括functions）
- 如果value是object，所有keys要按照相同的定义
- 如果key的前缀是	`typeof`，则只会为typeof定义

用`'"prodution"'`，或`JSON.stringify('production')`

参考资料：

- [vue-loader](https://vue-loader.vuejs.org/zh/#vue-loader-%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F)
- [autoprefixer](https://www.npmjs.com/package/autoprefixer)
- [详解vue-loader在项目中是如何配置的](https://www.jb51.net/article/141413.htm)
- [css-loader](https://www.webpackjs.com/loaders/css-loader/)