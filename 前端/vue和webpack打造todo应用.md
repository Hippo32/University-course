# Vue+Webpack打造todo应用 #
学习时间：两天

学习地址：[https://www.imooc.com/learn/935](https://www.imooc.com/learn/935)

2018/10/31 9:19:08 

## 搭建项目 ##
新建一个项目文件夹，然后

	npm init
生成一个package.json文件。

需要安装的项目以依赖项

- webpack
- vue
- vue-loader：是一个webpack的loader，它允许你以单文件.vue的格式撰写Vue组件。

		module: {
			rules: [
				// 其他规则
				{
        			test: /\.vue$/,
            		loader: 'vue-loader'
        		}
			]
		}