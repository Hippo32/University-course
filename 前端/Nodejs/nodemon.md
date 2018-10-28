# nodemon #
## 安装 ##
	npm install -g nodemon
Options

- --config file：alternate nodemon.json config file to use
- -e，--ext：extensions to look for， ie. js, jade, hbs
- -x, --exec app：execute script with "app", ie. -x "python -v".
- -w, --watch path：watch directory "path" or files. use once for each directory or file to watch.
- -i, --ignore：ignore specific files or directories.
- -V,--verbose：show detail on what is causing restarts.
- --<your args>：to tell nodemon stop slurping argument.

## 在express框架中引入nodemon ##
在项目目录下创建nodemon.json文件
	
	"restartable": "rs", // 重启的命令，默认是rs，可以改成你自己喜欢的字符串。当用nodemon启动应用时，可以直接键入rs直接重启服务。除了字符串值外，还可以设置false值，这个值的意思是当nodemon影响了你自己的终端命令时，设置为false则不会在nodemon运行期间监听rs的重启命令。
	"ignore": [ // 忽略的文件后缀名或者文件夹，文件路径的书写用相对于nodemon.json所在位置的相对路径
        ".git",
        ".svn",
        "node_modules/**/node_modules"
    ],
	"verbose": true, 设置日志输出模式，true详细模式
	"execMap": { // 运行服务的后缀名和对应的运行命令
		"": "node", // 指www这些没有后缀名的文件
        "js": "node --harmony" // 表示用nodemon 代替 node --harmony运行js后缀文件；
    },
	"watch": [ // 监听哪些文件的变化，当变化的时候自动重启

	],
	"env": { // 运行环境 
        "NODE_ENV": "development",  // 开发环境， production是生产环境。
		"PORT": "3000"
    },
	"ext": "js json" // 监控指定的后缀文件名

在package.json中修改

	"scripts": {
		"start": "node ./bin/www",
		"auto": "nodemon ./bin/www"
	},

参考文档：

- [https://www.cnblogs.com/chris-oil/p/6239097.html](https://www.cnblogs.com/chris-oil/p/6239097.html)
- [https://blog.csdn.net/a419419/article/details/78831869](https://blog.csdn.net/a419419/article/details/78831869)