# VUE中的导入导出 #
## 导入组件 ##
	import name1 from './name1'
	import name2 from './name2'
	
	export default {
		// ...
		components: {
			name1,
			name2
		}
		// ...
	}
## 导入CSS文件 ##
	import './test.css'
如果是在.vue文件中导入css

	<style>
	@import './test.css';
	
	</style>

## 小问题or小知识点 ##
	import { Tooltip } from 'element-ui'
`{}`就是ES6中的解构赋值，也就是说`element-ui`中可能有很多个`export`，现在引用的只是里面的其中一个。

## 一个export default{}的例子 ##
	export default {
		name: 'app', //可写可不写，写了可以提供更好的调试信息