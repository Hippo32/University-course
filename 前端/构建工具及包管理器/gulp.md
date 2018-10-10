# Gulp #
gulp是基于Nodejs的自动任务运行器，它能自动化地完成javascript/sass/less/html/image/css 等文件的的测试、检查、合并、压缩、格式化、浏览器自动刷新、部署文件生成，并监听文件在改动后重复指定的这些步骤。

使用Gulp,可以避免浏览器缓存机制，性能优化（文件合并，减少http请求；文件压缩）以及效率提升（自动添加CSS3前缀；代码分析检查）

## 安装 ##
需要全局安装和在项目目录下安装

	npm install -g gulp
	// 把目录切换到你的项目文件夹中
	npm install gulp --save-dev

## 使用 ##
### 建立gulpfile.js文件 ###
	var gulp = require('gulp');
	gulp.task('default', function() {
		console.log('hello world');
	});


### 运行gulp任务 ###
切换到存放`gulpfile.js`文件的目录，然后在命令行中执行`gulp`命令，`gulp`后面可以加上要执行的任务名，例如`gulp  task1`，如果没有指定任务名，则会执行任务名为`default`的默认任务。

## gulp的API ##
### gulp.task() ###
`gulp.task`方法用来定义任务，内部使用的是Orchestrator，其语法为：

	gulp.task(name[, deps], fn)

**name**为任务名

**deps**是当前定义的任务需要依赖的其他任务，为一个数组。当前定义的任务会在所有依赖的任务执行完毕后才开始执行。如果没有依赖，则可省略这个参数。

**fn**为任务函数，我们把任务要执行的代码都写在里面。该参数也是可选的。

	gulp.task('mytask', ['array', 'of', 'task', 'name'], function() { //定义一个有依赖的任务
		// do something
	});

### gulp.src() ###
Grunt主要是以文件为媒介来运行它的工作流的，比如在Grunt中执行完一项任务后，会把结果写入到一个临时文件后，然后可以在这个临时文件内容的基础上执行其他任务，执行完成后又把结果写入到临时文件中，然后又以这个为基础继续执行其他任务...就这样反复下去。而在Gulp中，使用的是Nodejs中的**stream（流）**，首先获取到需要的stream，然后可以通过stream的`pipe()`方法把流导入到你想要的地方，比如Gulp的插件中，经过插件处理后的流又可以继续导入到其他插件中，当然也可以把流写入到文件中。所以Gulp是以stream为媒介的，它不需要频繁的生成临时文件。

`gulp.src()`方法正是用来获取流的，但要注意这个流里的内容不是原始的文件流，而是一个虚拟文件对象流，这个虚拟文件对象中存储着原始文件的路径、文件名、内容等信息。可以用这个方法来读取你需要操作的文件。

	gulp.src(globs[, options])

**globs**参数是文件匹配模式（类似正则表达式），用来匹配文件路径（包括文件名），当然这里也可以直接指定某个具体的文件路径。当有多个匹配模式时，该参数可以为一个数组。

**options**为可选参数。通常情况下我们不需要用到。

### gulp.dest() ###
gulp.dest()方法是用来写文件的，其语法为：
	
	gulp.dest(path[,options])

**path**为写入文件的路径

**options**为一个可选的参数对象，通常我们不需要用到。

gulp的使用流程一般是这样子的：首先通过`gulp.src()`方法获取到我们想要处理的文件流，然后把文件通过pipe方法导入到gulp的插件中，最后把经过插件处理后的流再通过pipe方法导入到`gulp.dest()`中，`gulp.dest()`方法则把流中的内容写入到文件名，这里首先最需要弄清楚的一点是，我们给`gulp.dest()`传入的路径参数，只能用来指定要生成的文件的目录，而不能指定生成文件的文件名，它生成文件的文件名使用的是导入到它的文件流自身的文件名，所以生成的文件名是由导入到它的文件流决定的，即使我们给它传入一个带有文件名的路径参数，然后它也会把这个文件名当做是目录名，例如：

	var gulp = require('gulp');
	gulp.src('script/jquery.js')
		.pipe(gulp.dest('dist/foo.js'));
	// 最终生成的文件路径为dist/foo.js/jquery.js，而不是dist/foo.js

`gulp.dest(path)`生成的文件路径是我们传入的path参数后面再加上`gulp.src()`中有通配符开始出现的那部分路径。

### gulp.watch() ###
`gulp.watch()`用来监视文件的变化，当文件发生变化后，我们可以利用它来执行相应的任务，例如文件压缩等。其语法为：

	gulp.watch(glob[, opts], tasks)

**glob**为要监视的文件匹配模式，规则和用法与`gulp.src()`方法中的`glob`相同。

**opts**为一个可选的配置对象，通常不需要用到

**tasks**为文件变化后要执行的任务，为一个数组。

`gulp.watch()`还有另外一种是用方式：

	gulp.watch(glob[, opts, cb])
**glob** 和 **opts** 参数与第一种用法相同

**cb**参数为一个函数。每当监视的文件发生变化时，就会调用这个函数，并且会给它传入一个对象，该对象包含了文件变化的一些信息，`type`属性为变化的类型，可以是`added`，`changed`，`deleted`，`path`属性为发生变化的文件的路径。

参考文档：

- [前端构建工具gulpjs的使用介绍及技巧](https://www.cnblogs.com/2050/p/4198792.html)