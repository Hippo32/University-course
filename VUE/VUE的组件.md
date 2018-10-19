# Vue的组件介绍 #
例子：

	<div id="app">
	  <button-counter></button-counter>
	</div>
	<script type="text/javascript">
	Vue.component('button-counter', {
	  data: function(){
	    return {
	      count: 0
	    }
	  },
	  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
	})
	new Vue({
	  el: '#app'
	})

可接收与`new Vue`相同的选项，例如`data`、`computed`、`watch`、`methods`以及生命周期钩子等。仅有的例外是像`el`这样根实例特有的选项。

**注意**：

- `data`必须是一个函数。
- 每个组件必须只有一个根元素。可以将模板的内容包裹在一个父元素内。
- `Vue.component({})`要写在`new Vue({}`前面。
- 在遇到`<ul>`、`<ol>`、`<table>`和`<select>`时，它们的内部必须是`<li>`、`<tr>`和`<option>`，所以直接写自定义组件在里面会出错。但是可以用`is`解决。

		<table>
			<tr is="blog-post-row"></tr>
		</table>
	需要注意的是如果我们从以下来源使用模板的话，这条限制是不存在的：
	
	- 字符串（例如：`template: '...'`）
	- 单文件组件（`.vue`）
	- `<script type="text/x-template">`
## 组件注册 ##
### 组件名字 ###
- 使用kebab-case
- 使用PascalCase

### 全局注册 ###
	Vue.component('my-component-name', {})
### 局部注册 ###
	var ComponentA = {/* ... */}
	new Vue({
		el: '#app',
		components: {
			'component-a`: ComponnetA
		}
	})
对于`components`对象中的每个属性来说，其属性名就是自定义元素的名字，其属性值就是这个组件的选项对象。

## 组件里的数据传递 ##
### 用prop向子组件传递数据 ###
prop可以让你在组件上注册一些自定义特性。当一个值传递给自定义的那个prop特性的时候，它就变成了那个组件实例的一个属性。

	Vue.component('blog-post', {
		props: ['title'],
		template: '<h3>{{ title }}</h3>'
	})

	<blog-post title="My journey with Vue"></blog-post> //可以用v-bind动态传递prop

> 如果props在Vue.component中采用的是camelCase（驼峰命名法），则在HTML中要用kebab-case（短横线分隔命名）命名。

	Vue.component('blog-post', {
		props: ['postTitle'],
		template: '<h3>{{ postTitle }}</h3>'
	})
	
	<blog-post post-title="hello"></blog-post>
>如果你使用字符串模板，那么这个限制就不存在了。

**prop类型**

- 字符串数组形式
- 对象形式：key：属性名称，value：类型（例：`title: String`）

**传递prop**

根据测试，静态传prop，不管传什么类型，都会被vue当成字符串。所以如果要传除了字符串的其它类型，都要用`v-bind`

静态传prop：`value="23"`

动态传prop：`value="checked"`，Vue实例的data那里要声明了`checked`。

- 传入数字：`:value="23"`，虽然是静态变量，也要用`v-bind`，不加的话，`{{ typeof value }}`输出的值为`String`，加了的话输出`Number`。
- 传入布尔值
	- 不给属性赋值，意味着`true`

	        <div id="app">
                <test value></test>
	        </div>
	        <script>
	            Vue.component('test', {
	                props: {value: Boolean},
	                template: `<p>{{value.toString()}}</p>`
	            })
	            var vm = new Vue({el: "#app"})
	        </script>
			// 输出true
- 传入数组
- 传入对象：可以传单独一个属性，也可以传一个对象的所有属性

prop传值属于单向传值，只能由父级传向子组件。

----------


### 通过事件向父级组件发送消息 ###
在子组件中用`$emit`方法传入事件的名字，来向父级事件触发一个事件。第一个参数是事件的名字，第二个参数可以是要传给父组件的参数。

	<button v-on:click="$emit('enlarge-text', 0.1)">...

父级可以用`v-on`监听这个事件。可以用`$event`访问到子组件传过来的值。如果这个事件处理函数是一个函数，则这个值会作为第一个参数传入这个参数。


例子：

	<div id="app">
        <test 
			:style="{fontSize: postFontSize+'em'}" 
			v-for="(item, index) in arr" 
			:text="item.name" :value="item.age" 
			:key="item.id" 
			@enlarge-text="postFontSize += 0.1">
		</test>
    </div>
    <script>
        Vue.component('test', {
            props: ['text', 'value'],
            template: `
                <div>
                    <p>{{text}}</p>
                    <button @click="$emit('enlarge-text')">Enlarge text</button>
                </div>
            `
        })
        var vm = new Vue({
            el: "#app",
            data: {
                postFontSize: 1,
                arr: [
                    { id: 1, name: "xiaoming", age:20 },
                    { id: 2, name: "xiaohua", age: 18 },
                    { id: 3, name: "honghong", age: 8 }
                ]
            }
        })
    </script>

## 在组件里用v-model ##
子组件的input里要完成的事：

- 将其`value`特性绑定到一个名叫`value`的prop上
- 在其`input`事件被触发时，将新的值通过自定义的`input`事件抛出

例子：

    <div id="app">
        <custom-input v-model="searchText"></custom-input>
        <p>{{searchText}}</p>
    </div>
    <script>
        Vue.component('custom-input', {
            props: ['value'],
            template: `
                <input 
                    @input="$emit('input', $event.target.value)" 
                    :value="value"
                >
            `
        })
        new Vue({
            el: "#app",
            data: function(){
                return {
                    searchText: ''
                }
            }
        })
    </script>

## 组件的风格指南 ##
- 只要能够拼接文件的构建系统，就把每个组件单独分成文件。
- 单文件组件的文件名要么始终是单词大写开头，要么始终是横线连接。（MyComponent 或 my-component)
- 应用特定样式和约定的基础组件（也就是展示类的、无逻辑的或无状态的组件）应该全部以一个特定的前缀开头，比如**Base、App、v**。（例：BaseButton、AppButton、VButton）
- 只应该拥有单个活跃实例的组件应该以**The**前缀命名，以示其唯一性。
	- 这不意味着组件只可用于一个单页面，而是每个页面只使用一次。这些组件永远不接受任何prop，因为它们是为你的应用定制的，而不是它们在你的应用中的上下文。如果你发现有必要添加prop，那就表明这实际上是一个可复用的组件，只是目前在每个页面例只使用一次。
- 和父组件紧密耦合的子组件应该以父组件名作为前缀命名。

	例子：

		components/
		|- TodoList.vue
		|- TodoListItem.vue
		|- TodoListItemButton.vue
- 组件名应该以高级别的（通常是一般化描述的）单词开头，以描述性的修饰词结尾。

	例子：
	
		components/
		|- SearchButtonClear.vue
		|- SearchButtonRun.vue
		|- SearchInputExcludeGlob.vue
		|- SearchInputQuery.vue
		|- SettingsCheckboxLaunchOnStartup.vue
		|- SettingCheckboxTerms.vue
	最好在有100+个组件的时候才分目录。
- 在单文件组件、字符串模板和JSX中没有内容的组件应该是自闭和的———但在DOM模板里永远不要这样做。
- 对于绝大多数项目来说，在单文件组件和字符串模板中组件名应该总是PascalCase的，但是在DOM模板中总是kebab-case的。
- JS/JSX中的组件名应该始终是PascalCase的，尽管在较为简单的应用中只使用`Vue.component`进行全局组件注册时，可以使用kebab-case字符串。
- 组件名应该倾向于完整单词而不是缩写。

参考：

- [风格指南](https://cn.vuejs.org/v2/style-guide/#%E5%9F%BA%E7%A1%80%E7%BB%84%E4%BB%B6%E5%90%8D-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)
- PascalCase：第一个单字首字母采用大写字母；后续单字首字母亦用大写字母。例：FirstName、LastName
- kebab-case：短横线命名

## [单文件组件](https://cn.vuejs.org/v2/guide/single-file-components.html) ##
文件扩展名为`.vue`

例子：

	<template>
		<p>{{ greeting }}</p>
	</template>

	<script>
	module.exports = {
		data: function () {
			return {
				greeting: 'Hello'
			}
		}
	}
	</script>

	<style scoped>
	p {
		font-size: 2em;
		text-align: center;
	}
	</style>

**优点**

1. 完整语法高亮
2. CommonJS模块
3. 组件作用域的CSS
4. 可以使用预处理器来构建简洁和功能更丰富的组件，比如Pug，Babel和Stylus

## 字符串模板 ##
指的是在组件选项里用`template:""`指定的模板。

	var vm = new Vue({
		template: "<myComponent></myComponent>"
	});