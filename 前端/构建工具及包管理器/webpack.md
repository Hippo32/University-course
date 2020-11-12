# webpack #

上面的例子参考的是，下面的例子参考的是[入门webpack，看这篇就够了](https://www.jianshu.com/p/42e11515c10f "入门webpack，看这篇就够了")



### loader ###
webpack自身只支持JavaScript。而loader能够让webpack处理那些非JavaScript文件，并且先将它们转换为有效模块，然后添加到依赖图中，这样就可以提供给应用程序使用。

loader用于对模块的源代码进行转换。loader可以使你在`import`或“加载”模块时预处理文件。

在webpack的配置中loader有两个特征：

1. `test`属性，用于标识出应该被对应的loader进行转换的某个或某些文件。
2. `use`属性，表示进行转换时，应该使用哪个loader。

		module: {
			rules: [
				{
					test: /\.txt$/,
					use: 'raw-loader'
				}
			]
		}
	在遇到`.txt`结尾的路径时，在打包之前，先使用`raw-loader`转换一下。

在你的应用程序中，有三种使用loader的方式：

- 配置（推荐）：在webpack.config.js文件中指定loader。
	- `module.rules`允许你在webpack配置中指定多个loader。
- 内联：在每个`import`语句中显式指定loader。
	- 使用`!`将资源中的loader分开。分开的每个部分都相对于当前目录解析。

			import Styles from 'style-loader!css-loader?modules!./styles.css';
		通过前置所有规则及使用`!`，可以将源文件对应重载到配置中的任意loader中。
- CLI：在shell命令行中指定它们。

		webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
	这会对`.jade`文件使用`jade-loader`，对`.css`文件使用`style-loader`和`css-loader`。

参考：[https://webpack.docschina.org/concepts/loaders](https://webpack.docschina.org/concepts/loaders)

### 插件（plugins） ###

想要使用一个插件，你只需要`require()`它，然后把它添加到`plugins`数组中。多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这是需要通过使用`new`操作符来创建它的一个实例。

	// webpack.config.js
	const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过npm安装
	const webpack =  require('webpack'); // 用于访问内置插件

	module.exports = {
		module: {
			rules: [
				{ test: /\.txt$/, use: 'raw-loader' }
			]
		},
		plugins: [
			new HtmlWebpackPlugin({template: './src/index.html'})
		]
	};
在上面的实例中，`html-webpack-plugin`会为你的应用程序生成一个html文件，然后自动注入所有生成的bundle。

参考：[webpack提供的插件](https://webpack.docschina.org/plugins)


## 模式（mode） ##
通过将`mode`参数设置为`development`，`production`或`none`，可以启用对应环境下webpack内置的优化。默认值为`production`。
	
	module.exports = {
		mode: 'production'
	};

支持以下字符串值：

|选项|描述|
|---|----|
|`development`|会将`process.env.NODE_ENV`的值设为`development`。启用`NamedChunksPlugin`和`NamedModulesPlugin`。|
|`production`|会将`process.env.NODE_ENV`的值设为`production`。启用`FlagDependencyUsagePlugin`，`FlagIncludeChunksPlugin`，`ModuleConcatenationPlugin`，`NoEmitOnErrorsPlugin`，`OccurrenceOrderPlugin`，`SideEffectsFlagPlugin`和`UglifyJsPlugin`。|
|`none`|不选用任何默认优化选项|

如果不设置，webpack会将production作为mode的默认值去设置。

> 记住，只设置`NODE_ENV`时，不会自动设置`mode`。

如果想要根据webpack.config.js中的mode变量去影响编译行为，那你必须将导出对象，改为导出一个函数：

	var config = {
		entry: './app.js'
		// ...
	};

	module.exports = (env, argv) => {
		if(argv.mode === 'development') {
			config.devtool = 'source-map';
		}
		
		if(argv.mode === 'production') {
			//...
		}

		return config;
	};



# Webpack的强大功能 #
## 使用webpack构建本地服务器 ##
webpack提供一个可选的本地开发服务器，这个本地服务器基于node.js构建，可以让你的浏览器监听你的代码的修改，并自动刷新显示修改后的结果，不过它是一个单独的组件，在webpack中进行配置之前需要单独安装它作为项目依赖

    npm install --save-dev webpack-dev-server

|devsserver的配置选项 | 功能描述|
|------------------- |:-------:|
|contentBase|默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录（本例设置到“public"目录）|
|port|设置默认监听端口，如果省略，默认为“8080”|
|inline|设置为`true`，但源文件改变时会自动刷新页面|
|historyApiFallback|在开发单页应用时非常有用，它依赖于HTML5historyAPI，如果设置为true，所有的跳转将指向index.html|
把这些命令加到webpack的配置文件中，现在的配置文件`webpack.config.js`如下图所示
    
    module.exports = {
	devtool: 'eval-source-map',

	entry:__dirname + "/app/main.js",// 已多次提及的唯一入口文件
	output: {
		path: __dirname + "/public",// 打包后的文件存放的地方
		filename: "bundle.js"// 打包后输出文件的文件名
	},

	devServer: {
		contentBase: "./public",//本地服务器所加载的页面所在的目录
		historyApiFallback: true,//不跳转
		inline:true
	}
	}
在`package.json`中的`scripts`对象中添加如下命令，用以开启本地服务器：

    "scripts": {
		"test": "echo \"Error: no test specified\" && exit 1",
		"start": "webpack",
		"server": "webpack-dev-server --open"
	},
在终端中输入`npm run server`即可在本地的`8080`端口查看结果

## Loaders ##
通过使用不同的`loader`，`webpack`有能力调用外部的脚本或工具，实现对不同格式的文件的处理，比如说分析转换scss为css，或者把下一代的JS文件（ES6，ES7)转换为现代浏览器兼容的JS文件，对React的开发而言，合适的Loaders可以把React的中用到的JSX文件转换为JS文件。

Loaders需要单独安装并且需要在`webpack.config.js`中的`modules`关键字下进行配置，Loaders的配置包括以下几方面：

- `test`：一个用以匹配loaders所处理文件的拓展名的正则表达式（必须）
- `loader`：loader的名称（必须）
- `include/exclude`:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）
- `query`：为loaders提供额外的设置选项（可选）

**例子：添加处理css文件需要的两个Loader**

1. 在项目目录下线安装处理css文件需要的两个Loader

    	npm install style-loader css-loader --save-dev
2. 在webpack.config.js中修改，教会webpack用什么Loader处理什么类型的文件
	
	    module.exports = {
    		// 入口文件名称
   			entry: './index.js',
    		// 输出文件名称
    		output: {
        		filename: 'bundle.js'
    		},
    		// 这里是我们新添加的处理不同类型文件需要的 Loader
    		module: {
    			rules: [
    				{ test: /\.css$/,
					  use: [ 
						{loader: 'style-loader'}, 
						{ loader: 'css-loader' } 
					]}
    			]
    		}
		}
仔细分析一下这一句语法：

		{ test: /\.css$/, use: [ { loader: 'style-loader' }, { loader: 'css-loader' } ]}
前面test传入一个正则表达式，代表你要处理什么类型的文件，这里指的是后缀名为.css的文件，后面的use传入一个数组，代表着这个文件龟背这两个Loader所处理，一个文件可以被多个Loader所处理。

ps：style-loader作用是将样式动态的使用js插入到页面中去，css-loader使你能够使用类似@import 和 url(...)的方法实现 require()的功能。webpack的特点之一：任何类型的模块（资源文件），理论上都可以通过被转化为JavaScript代码实现与其他模块的合并与加载

在官方网站找到各种webpack的loaders列表：[Webpack Loader List](https://webpack.js.org/loaders/ "Webpack Loader List")
## 插件（Plugins） ##
要使用某个插件，我们需要通过`npm`安装它，然后要做的就是在webpack配置中的plugins关键字部分添加该插件的一个实例（plugins是一个数组）。

**常用的插件**

**HtmlWebpackPlugin**

这个插件的作用是依据一个简单的`index.html`模板，生成一个自动引用你打包后的JS文件的新`index.html`。这在每次生成的js文件名称不同时非常有用（比如添加了`hash`值）。

**安装**

    npm install --save-dev html-webpack-plugin

这个插件自动完成了我们之前手动做的一些事情，在正式使用之前需要对一直以来的项目结构做一些更改：

1. 移除public文件夹，利用此插件，index.html文件会自动生成，此外CSS已经通过前面的操作打包到JS中了。
2. 在app目录下，创建一个`index.tmpl.html`文件模板，这个模板包含`title`等必须元素，在编译过程中，插件会依据此模板生成最终的`html`页面，会自动添加所依赖的 css, js，favicon等文件，`index.tmpl.html`中的模板源代码如下：


		<!DOCTYPE html>
		<html>
		<head lang="en">
			<meta charset="utf-8">
			<title>Webpack Sample Project</title>
		</head>
		<body>
			<div id="root">
			</div>
		</body>
		</html>
-----

**Hot Module Replacement**
`Hot Module Replacement`（HMR）允许你在修改组件代码后，自动刷新实时预览修改后的效果。

在webpack中实现HMR也很简单，只需要做两项配置

- 在webpack配置文件中添加HMR插件；
- 在Webpack Dev Server中添加“hot”参数；

不过配置完这些后，JS模块其实还是不能自动热加载的，还需要在里的JS模块中执行一个Webpack提供的API才能实现加载，虽然这个API不难使用，但是如果是React模块，使用我们已经熟悉的Babel可以更方便的实现功能热加载。

具体实现方法如下：

- `Babel`和`webpack`是独立的工具
- 二者可以一起工作
- 二者都可以通过插件扩展功能
- HMR是一个webpack插件，它让你能在浏览器中实时观察模块修改后的效果，但是如果你想让它工作，需要对模块进行额外的配额；
- Babel有一个叫做`react-transform-hrm`的插件，可以在不对React模块进行额外的配置的前提下让HMR正常工作；

配置

	const webpack = require('webpack');
	const HtmlWebpackPlugin = require('html-webpack-plugin');
	module.exports = {
		entry: __dirname + "/app/main.js",// 已多次提及的唯一入口文件
		output: {
			path: __dirname + "/public",// 打包后的文件存放的地方
			filename: "bundle.js"// 打包后输出文件的文件名
		},
		devtool: 'eval-source-map',//使调试更容易
		devServer: {
			contentBase: "./public",//本地服务器所加载的页面所在的目录
			historyApiFallback: true,//不跳转
			inline:true//实时刷新
		},
		module: {
			rules: [
				{
					// test 和 include 具有相同的作用，都是必须匹配选项
					// exclude 是必不匹配选项（优先于test和include）
					// 最佳实践：
					// - 只在test和文件名匹配中使用正则表达式
					// - 在include和exclude中使用绝对路径数组
					// - 尽量避免exclude，更倾向于使用include
					test: /(\.jsx|\.js)$/,
					use: {
						loader: "babel-loader",
					},
					exclude: /node_modules/
				},
				{
					test: /\.css$/,
					use: [
						{
							loader: "style-loader"
						},{
							loader: "css-loader",
							options: {
								modules: true,// 指定启用css modules
								localIdentName: '[name]__[local]--[hash:base64:5]' // 指定css的类名格式
							},
						},{
								loader: "postcss-loader"
							}
					]
				}
			]
		},
		plugins: [
			new webpack.BannerPlugin('版权所有，翻版必究'),
			new HtmlWebpackPlugin({
			template: __dirname + "/app/index.tmpl.html"//new 一个这个插件的实例，并传入相关的参数
		}),
			new webpack.HotModuleReplacementPlugin()//热加载插件
		],
	};

安装`react-transform-hmr`

	npm install --save-dev babel-plugin-react-transform react-transform-hmr

配置Babel

	// .babelrc 
	{ 
		"presets": ["react", "env"], 
		"env": { 
			"development": { 
			"plugins": [["react-transform", { 
				"transforms": [{ 
					"transform": "react-transform-hmr", 
					"imports": ["react"],
					 "locals": ["module"] 
					}] 
				}]] 
			} 
		} 
	}

现在当你使用React时，可以热加载模块了，每次保存就能在浏览器上看到更新内容。

参考文章：

- [入门webpack，看这篇就够了](https://www.jianshu.com/p/42e11515c10f "入门webpack，看这篇就够了")
- [webpack 配置](https://webpack.docschina.org/configuration)
- [webpack 命令行接口](https://webpack.docschina.org/api/cli/)
- [webpack 模块方法](https://webpack.docschina.org/api/module-methods)