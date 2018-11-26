# ES6的模块导入导出 #
## ES6 import 和 export 在浏览器当中的使用 ##
1. 显式声明`type="module"`

	> script里面要加type="module"，这样浏览器才会把相关的代码当作ES6的module来对待。

		<script type="module">
			import {firstName} from './utils.js';
			console.log(firstName);
		</script>
2. 不能写“裸”路径如：

		<script type="module">
			import {firstName} from 'utils.js'; // error
			console.log(firstName);
		</script>
	直接写'util.js'会报错  
	你可以写绝对路径和相对路径，但是不能直接写文件名，即使是同一层级下面的文件。也要加上'./name.js'。

参考文章：

- [ES6 import 和 export 在浏览器当中的使用](https://segmentfault.com/a/1190000014342718)