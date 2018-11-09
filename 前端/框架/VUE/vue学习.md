# Vue #
## Vue安装 ##
### 1. 直接用`<script>`引入 ###
直接下载并用`<script>`标签引入，Vue会被注册为一个全局变量。

	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
可以在[https://cdn.jsdelivr.net/npm/vue/](https://cdn.jsdelivr.net/npm/vue/)浏览npm包的源代码。
### 2.NPM ###
	# 最新稳定版
	npm install vue
### 3.命令行工具CLI ###
用vue-cli
### 开发版本和生产版本的区别 ###
- 开发版本：包含了完整的警告和调试模式，开发环境下用未压缩的代码
- 生产版本：删除了警告，生产环境下使用压缩后的代码

> 如果要用`template`，就需要加上编译器，即完整版。
## 什么是Vue ##
Vue是一个基于MVVM模式数据驱动页面的框架，它将数据绑定在视图上。属于实现单页面应用的技术。总结起来的几大特点：

1. 简洁
2. 轻量
3. 快速
4. 数据驱动
5. 模块友好
6. 组件化

Vue靠数据驱动双向绑定使我们开发页面更简单，开发者不需要手动的去修改dom。Vue通过数据双向绑定使一切变得更简单。它的数据驱动双向绑定，底层是通过`Object.defineProperty()`定义的数据set、get函数原理实现。

组件化开发：让项目的可扩展性、移植性更好，代码重用性更高。

单页面应用的体验，局部组件更新界面，让用户体验更快速省时。

> 在vue.js中，所谓的数据驱动就是当数据发生变化的时候，用户界面发生相应的变化，开发者不需要手动的去修改dom。

## Vue.js是如何实现数据驱动的？ ##
首先，vuejs在实例化的过程中，会对实例化对象选项中的data选项进行遍历，遍历其所有属性并使用`Object.defineProperty`把这些属性全部转为`getter/setter`。同时每一个实例对象都有一个`watcher`实例对象，他会在模板编译的过程中，用getter去访问data的属性，watcher此时就会把用到的data属性记为依赖，这样就建立了视图与数据之间的联系。当之后我们渲染视图的数据依赖发生了变化（即数据的setter被调用）的时候，watcher会对比前后两个的数值是否发生变化，然后确定是否通知视图进行重新渲染，这样就实现了所谓的数据对于视图的驱动。通俗地讲，它意味着我们在普通HTML模板中使用特殊的语法将DOM“绑定”到底层数据。一旦创建了绑定，DOM将与数据保持同步。每当修改了数据，DOM便相应地更新。这样我们应用中的逻辑就几乎都是直接修改数据了，不必与DOM更新搅在一起。这让我们的代码更容易撰写、理解与维护。

应用程序的数据比较复杂时，推荐用vue
## 创建一个Vue实例 ##
每个Vue应用都是通过用`vue`函数创建一个新的vue实例开始的：

	var vm = new Vue({
		// 选项
	})
Vue组件都是Vue实例，并且接受相同的选项对象（一些根实例特有的选项除外）。

## 数据与方法 ##
Vue会递归将data的属性转换为getter/setter，从而让data的属性能够响应数据变化。

只有当实例被创建时`data`中存在的属性才是响应式的。

	var vm = new Vue({
		data : {
			a : 1,
			b : 2
			todos : []
		}
	})
	vm.a // 1
	
**例外：使用`Object.freeze()`，阻止修改现有的属性，即数据无法响应变化。**

除了数据属性，Vue实例还暴露了一些有用的实例属性与方法。它们都有前缀`$`，以便与用户定义的属性区分开来。

常见[实例属性](https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B%E5%B1%9E%E6%80%A7)：

- vm.$data：Vue实例观察的数据对象。Vue实例代理了对其data对象属性的访问。
- vm.$props：当前组件接收到的props对象。Vue实例代理了对其props对象属性的访问。
- vm.$el：Vue实例使用的根DOM元素。
- vm.$options：用于当前Vue实例的初始化选项。

## 实例生命周期钩子 ##
每个Vue实例在被创建时都要经过一系列的初始化过程。在这个过程中会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会。

- `beforeCreate`
	- 类型：Function
	- 详细：在实例初始化之后，数据监测（data observer）和event/watcher事件配置之前被调用。
- `created`
	- 类型：Function
	- 详细：在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测（data observer），属性和方法的运算，watch/event事件回调。然而，挂载阶段还没开始，`$el`属性目前不可见。
- `beforeMount`
	- 类型：Function
	- 详细：在挂载开始之前被调用：相关的`render`函数首次被调用
	- 该钩子在服务器端渲染期间不被调用
- `mounted`
	- 类型：Function
	- 详细：`el`被创建的`vm.$el`替换，并挂载到实例上去之后调用该钩子。如果root实例挂载了一个文档内元素，当`mounted`被调用时`vm.$el`也在文档内。
	- 注意`mounted`不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用`vm.$nextTick`替换掉`mounted`；
	- 该钩子在服务器端渲染期间不被调用。
- `beforeUpdate`
	- 类型：Function
	- 详细：数据更新时调用，发生在虚拟DOM打补丁之前。这里适合在更新之前访问现有的DOM，比如手动移除已添加的事件监听器。
	- 该钩子在服务器渲染期间不被调用，因为只有初次渲染会在服务端进行。
- `updated`
	- 类型：Function
	- 详细：由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后会调用该钩子。当这个钩子被调用时，组件DOM已经更新，所以你现在可以执行依赖于DOM的操作。
- `activated`
	- 类型：Function
	- 详细：keep-alive组件激活时调用
- `deactivated`
	- 类型：Function
	- 详细：keep-alive组件停用时调用
- `beforeDestroy`
	- 类型：Function
	- 详细：实例销毁之前调用。在这一步，实例仍然完全可用。
- `destroyed`
	- 类型：Function
	- 详细：Vue实例销毁后调用。调用后，Vue实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
- `errorCaptured`
	- 类型：`(err: Error, vm: Component, info: string) => ?boolean`
	- 详细：当捕获一个来自子孙组件的错误时被调用。此钩子会收到三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回`false`以阻止该错误继续向上传播。

生命周期钩子的`this`上下文指向调用它的Vue实例。

## 模板语法 ##
在底层的实现上，Vue将模板编译成虚拟DOM渲染函数。结合响应系统，Vue能够智能地计算出最少需要渲染多少组件，并把DOM操作次数减到最少。

当Vue选项对象中有render渲染函数时，Vue构造函数将直接使用渲染函数渲染DOM树，当选项对象中没有render渲染函数时，Vue构造函数首先通过将template模板编译生成渲染函数，然后再渲染DOM树，而当Vue选项对象中既没有render渲染函数，也没有template模板时，会通过el属性获取挂载元素的outerHTML来作为模板，并编译生成渲染函数。

- **具名插槽**：就是将某个名字的内容插到子组件对应名字里面去

- **虚拟DOM**  
Vue通过建立一个虚拟DOM对真实DOM发生的变化保持追踪。

		return createElement('h1', this.blogTitle)
`createElement`其实不是一个实际的DOM元素。它更准确的名字是`createNodeDescription`，因为它所包含的信息会告诉Vue页面上需要渲染什么样的节点，及其子节点。我们把这样的节点描述为“虚拟节点”，也常简写为“VNode”。“虚拟DOM”是我们对由Vue组件树建立起来的整个Vnode树的称呼。

参考文章：[Vuejs2.0学习之二（Render函数,createElement，vm.$slots，函数化组件，模板编译，JSX）](https://blog.csdn.net/kkae8643150/article/details/52910389)

### 渲染函数 ###
	<blog-post>
		<h1 slot="header>
			About Me
		</h1>

		<p>Here's some page content, which will be included in vm.$slots.default, because it's not inside a named slot.</p>

		<p slot="footer">
			Copyright 2016 Evan You
		</p>
	
		<p>If I have some content down here, it will also be included in vm.$slots.default.</p>
	</blog-post>

	Vue.component('blog-post', {
		render: function (createElement) {
			var header = this.$slots.header
			var body = this.$slots.default
			var footer = this.$slots.footer
			return creatElement('div', [
				createElement('header', header),
				createElement('main', body),
				createElement('footer', footer)
			])
		}
	})


### 指令 ###
指令是带有`v-`前缀的特殊特性。指令特性的值预期是**单个JavaScript表达式**（`v-for`是例外情况）。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于DOM。

- 参数：一些指令能够接收一个“参数”，在指令名称之后以冒号表示。
- 修饰符：以半角句号`.`指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。

### 缩写 ###
`v-`前缀作为一种视觉提示，用来识别模板中Vue特定的特性。

- `v-bind`缩写
		
		// 完整语法
		<a v-bind:href="url">...</a>

		// 缩写
		<a :href="url">...</a>
- `v-on`缩写

		// 完整语法
		<a v-on:click="doSomething">...</a>

		// 缩写
		<a @click="doSomething>...</a>

## 计算属性和侦听器 ##
### 计算属性 ###
应用场景：对于任何复杂逻辑，都应当使用计算属性。当有一些数据需要随着其他数据变动而变动时，比起用`watch`，更好的做法是使用计算属性。

可以像绑定普通属性一样在模板中绑定计算属性。

	var vm = new Vue({
		el: '#example',
		data: {
			message: 'Hello'
		},
		computed: {
			reversedMessage: function() {
				return this.message.split('').reverse().join('')
			}
		}
	})
可以通过在表达式中调用方法来达到同样的效果

	methods: {
		reversedMessage: function() {
			...
		}
	}
**两种方式的最终结果确实是完全相同的，不同的是计算属性是基于它们的依赖进行缓存的。**计算属性只有在它的相关依赖发生改变时才会重新求值。**每当触发重新渲染时，调用方法总会再次执行函数。**

计算属性默认只有getter，不过在需要时也可以提供一个setter：

	computed: {
		fullName : {
			//getter
			get : function() {
				return this.firstName + ' ' + this.lastName
			},
			// setter
			set : function (newValue) {
				var names = newValue.split(' ')
				this.firstName = names[0]
				this.lastName = names[names.length - 1]
			}
		}
	}

### 侦听器 ###
Vue通过`watch`选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

#### watch的用法： ####

watch的键是你要监控的东西。

**值可以是函数**：当你监控的东西变化时，需要执行的函数，这个函数有两个形参，第一个是当前值，第二个是变化前的值

**值也可以是函数名**：不过这个函数名要用单引号来包裹。

**值是包括选项的对象**：选项包括有三个。
	
1. 第一个**handler**：其值是一个回调函数。即监听到变化时应该执行的函数。
2. 第二个是**deep**：其值是true或false，确认是否深入监听。（一般监听时是不能监听到对象属性值的变化的，数组的值变化可以听到）
3. 第三个是**immediate**：其值是true或false；确认是否以当前的初始值执行handler的函数。


----------


1. 普通的watch
		
		data: {
			a : 10
		},
		watch: {
			a: function(newVal, oldVal) {
				console.log(newVal);
			}
		}
参考链接：[https://www.cnblogs.com/hity-tt/p/6677753.html](https://www.cnblogs.com/hity-tt/p/6677753.html)

## Class与Style绑定 ##
在将`v-bind`用于`class`和`style`时，Vue.js做了专门的增强。表达式结果的类型除了字符串之外，还可以是对象或数组。

### 绑定HTML Class ###
有对象语法和数组语法两种
#### 对象语法 ####
	<div class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>

	data: {
		isActive: true,
		hasError: false
	}
或者

	<div v-bind:class="classObject"></div>

	data: {
		classObject: {
			active: true,
			'text-danger': false
		}
	}

结果渲染为：
	
	<div class="static active"></div>

我们也可以绑定一个返回对象的计算属性。

	<div v-bind:class="classObject"></div>
	
	data: {
		isActive: true,
		error: null
	},
	computed: {
		classObject: function() {
			return {
				active: this.isActive && !this.error
			}
		}
	}
#### 数组语法 ####
	<div v-bind:class="[activeClass, errorClass]"></div>

	data: {
		activeClass: 'active',
		errorClass: 'text-danger'
	}
渲染为：
	
	<div class='active text-danger'></div>

如果想根据条件切换列表中的class，可以用三元表达式。当有多个条件class是这样写有些繁琐。所以在数组语法中也可以使用对象语法。

#### 用在组件上 ####
当在一个自定义组件上使用`class`属性时，这些类将被添加到该组件的根元素上面。这个元素上已经存在的类不会被覆盖。

### 绑定内联样式 ###
同样有对象语法和数组语法两种。
#### 对象语法 ####
CSS属性名可以用驼峰式（camelCase）或短横线分隔（kebab-case，记得用单引号括起来）来命名。

用法跟绑定class差不多。

#### 数组语法 ####
`v-bind:style`的数组语法可以将多个样式对象应用到同一个元素上。

	<div v-bind:style="[baseStyles, overridingStyles]"></div>

#### 自动添加前缀 ####
当`v-bind:style`使用需要添加浏览器引擎前缀的CSS属性时，如`transform`，Vue.js会自动侦测并添加相应的前缀。

#### 多重值 ####
2.3.0+

	<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>

## 条件渲染 ##
### `v-if` ###
#### 在`<template>`元素上使用`v-if`条件渲染分组 ####
因为`v-if`是一个指令，所以必须将它添加到一个元素上。如果想切换多个元素，可以把一个`<template>`元素当做不可见的包裹元素，并在上面使用`v-if`。最终的渲染结果将不包含`<template>`元素。
	
	<template v-if="ok">
		<h1>Title</h1>
		<p>Paragraph</p>
	</template>
### `v-else` ###
`v-else`元素必须紧跟`v-if`或者`v-else-if`的元素的后面，否则它将不会被识别。
### `v-else-if` ###
类似于`v-else`，`v-else-if`也不许紧跟在带`v-if`或者`v-else-if`的元素之后。
### `v-show` ###
带有`v-show`的元素始终会被渲染并保留在DOM中。`v-show`只是简单地切换元素的CSS属性`display`。

`v-show`不支持`<template>`元素，也不支持`v-else`。

### `v-if` VS `v-show` ###
`v-if`是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件是当地被销毁和重建。

`v-if`也是**惰性的**：如果在初始渲染时条件为假，则什么也不做一直到田间第一次为真时，才会开始渲染条件块。

相比之下，`v-show`不管初始条件是什么，元素总会被渲染，并且只是简单地基于CSS进行切换。

一般来说，`v-if`有更高的切换开销，而`v-show`有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用`v-show`较好；如果在运行时条件很少改变，则使用`v-if`较好。

当`v-if`与`v-for`一起使用时，`v-for`具有比`v-if`更高的优先级。

----------

# v-for #
## 列表渲染 ##
### 用`v-for`把一个数组对应为一组元素 ###
用`v-for`指令根据一组数组的选项列表进行渲染。`v-for`指令需要使用`item in items`形式的特殊语法，`items`是源数据数组并且`item`是数组元素迭代的别名。

`v-for`还支持一个可选的第二个参数为当前项的索引。

	<li v-for = "(item, index) in items">

也可以用`of`替代`in`作为分隔符

	<div v-for="item of items"></div>
### 一个对象的`v-for` ###
也可以用`v-for`通过一个对象的属性来迭代。
<br>也可以提供第二个的参数为键名，第三个参数为索引

	<div v-for="(value, key, index) in object">
		{{ index }}. {{ key }}: {{ value }}
	</div>
在遍历对象时，是按`Object.keys()`的结果遍历，但是不能保证它的结果是在不同的JS引擎下是一致的。

### `key` ###
当Vue.js用`v-for`正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue将不会移动DOM元素来匹配数据项的顺序，而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。

为了给Vue一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一`key`属性。理想的`key`值是每项都有的且唯一的id。需要用`v-bind`来绑定动态值。
	
	<div v-for="item in items" :key="item.id>
	</div>
建议尽可能在使用`v-for`时提供`key`，除非遍历输出的DOM内容非常简单，或者是刻意依赖默认行为（就地复用）以获取性能上的提升。

参考链接：[https://www.zhihu.com/question/61078310/answer/361261031](https://www.zhihu.com/question/61078310/answer/361261031)

### 数组更新检测 ###
#### 变异方法 ####
Vue包含一组观察数组的变异方法，所以它们也将会触发视图更新。这些方法如下：

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

变异方法：能改变原数组的方法

#### 替换数组 ####
非变异方法：`filter()`，`concat()`和`slice()`，这些不会改变原始数组，但总是返回一个新数组。当使用非变异方法时，可以用新数组替换旧数组。

	example1.items = example1.items.filter(function (item) {
	  return item.message.match(/Foo/)
	})

用一个含有相同元素的数组去替换原来的数组是非常有效的操作。

#### 注意事项 ####
由于JS的限制，Vue不能检测以下变动的数组：

1. 当你利用索引直接设置一个项时，例如：`vm.items[indexOfItem] = newValue`
2. 当你修改数组的长度时，例如：`vm.items.length = newLength`

以下两种方式都可以实现和`vm.items[indexOfItem] = newValue`相同的效果，同时也将触发状态更新：

	// Vue.set
	Vue.set(vm.items, indexOfItem, newValue)
	
	// Array.prototype.splice
	vm.items.splice(indexOfItem, 1, newValue)

解决`vm.items.length = newLength`，可以使用`splice`：

	vm.items.splice(newLength)

### 对象更改检测注意事项 ###
由于JS的限制，Vue不能检测对象属性的添加或删除

对于已经创建的实例，Vue不能动态添加根级别的响应式属性。但是可以使用`Vue.set(object, key, value)`方法向嵌套对象添加响应式属性。

还可以使用`vm.$set`实例方法，它只是全局`Vue.set`的别名。

有时可能需要为已有对象赋予多个新属性，比如使用`Object.assign()`或`_.extend()`。在这种情况下，你应该用两个对象的属性创建一个新的对象。

	vm.userProfile = Object.assign({}, vm.userProfile, {
		age: 27,
		favoriteColor: 'Vue Green'
	})

### 显示过滤/排序结果 ###
有时，我们想要显示一个数组的过滤或排序副本，而不实际改变或重置原始数据。在这种情况下，可以创建返回过滤或排序数组的计算属性。

	<li v-for="n in evenNumbers">{{ n }}</li>

	data: {
		numbers: [1, 2, 3, 4, 5 ]
	},
	computed: {
		evenNumbers: function() {
			return this.numbers.filter(function (number) {
				return number % 2 === 0
			})
		}
	}
在计算属性不适用的情况下（例如，在嵌套`v-for`循环中）你可以使用一个method方法：

	<li v-for="n in even(numbers)">{{ n }}</li>

	data: {
	  numbers: [ 1, 2, 3, 4, 5 ]
	},
	methods: {
	  even: function (numbers) {
	    return numbers.filter(function (number) {
	      return number % 2 === 0
	    })
	  }
	}

### 一段取值范围的`v-for` ###
`v-for`也可以取整数。在这种情况下，它将重复多次模板。

	<div>
	  <span v-for="n in 10">{{ n }} </span>
	</div>

	//输出：1 2 3 4 5 6 7 8 9 10

### `v-for` on a `<template>` ###
类似于`v-if`，你也可以利用带有`v-for`的`<template>`渲染多个元素。

	<ul>
	  <template v-for="item in items">
	    <li>{{ item.msg }}</li>
	    <li class="divider" role="presentation"></li>
	  </template>
	</ul>

### `v-for` with `v-if` ###
当它们处于同一节点，`v-for`的优先级比`v-if`更高，这意味着`v-if`将分别重复运行于每个`v-for`循环中。当你想为仅有的一些项渲染节点时，这种优先级的机制会十分有用。

	<li v-for="todo in todos" v-if="!todo.isComplete">
	  {{ todo }}
	</li>
而如果你的目的是有条件地跳过循环的执行，那么可以将`v-if`置于外层元素（或`<template>`）上，如：

	<ul v-if="todos.length">
	  <li v-for="todo in todos">
	    {{ todo }}
	  </li>
	</ul>
	<p v-else>No todos left!</p>

### 一个组件的`v-for` ###
在自定义组件里，可以像任何普通元素一样用`v-for`

	<my-component v-for="item in items" :key="item.id"></my-component>
然而，任何数据都不会被自动传递到组件里，因为组件有自己独立的作用域。为了把迭代数据传递到组件里，我们要用`props`：

	<my-component
	  v-for="(item, index) in items"
	  v-bind:item="item"
	  v-bind:index="index"
	  v-bind:key="item.id"
	></my-component>

不自动将`item`注入到组件里的原因是，这会使得组件与`v-for`的运作紧密耦合。明确组件数据的来源能够使组件在其他场合重复使用。

## 事件处理 ##
### 监听事件 ###
可以用`v-on`指令监听DOM事件，并在触发时运行一些JS代码。

### 事件处理方法 ###
`v-on`还可以接收一个需要调用的方法名称。

### 内联处理器中的方法 ###
除了直接绑定到一个方法，也可以在内联JS语句中调用方法。

有时也需要在内联语句处理器中访问原始的DOM事件。可以用特殊变量`$event`把它传入方法。

### 事件修饰符 ###
修饰符是由点开头的指令后缀来表示的。

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`：可用到自定义的组件事件上。
- `.passive`：能够提升移动端的性能。

> 不要把`.passive`和`.prevent`一起使用，因为`.prevent`将会被忽略，同时浏览器可能会向你展示一个警告。请记住，`.passive`会告诉浏览器你不想组织事件的默认行为。


----------


	<!-- 阻止单击事件继续传播 -->
	<a v-on:click.stop="doThis"></a>
	
	<!-- 提交事件不再重载页面 -->
	<form v-on:submit.prevent="onSubmit"></form>
	
	<!-- 修饰符可以串联 -->
	<a v-on:click.stop.prevent="doThat"></a>
	
	<!-- 只有修饰符 -->
	<form v-on:submit.prevent></form>
	
	<!-- 添加事件监听器时使用事件捕获模式 -->
	<!-- 即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理 -->
	<div v-on:click.capture="doThis">...</div>
	
	<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
	<!-- 即事件不是从内部元素触发的 -->
	<div v-on:click.self="doThat">...</div>

	// 2.1.4新增
	<!-- 点击事件将只会触发一次 -->
	<a v-on:click.once="doThis"></a>

	// 2.3.0新增
	<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
	<!-- 而不会等待 `onScroll` 完成  -->
	<!-- 这其中包含 `event.preventDefault()` 的情况 -->
	<div v-on:scroll.passive="onScroll">...</div>

参考文章：[vue 事件修饰符](https://blog.csdn.net/wxy270/article/details/79397076)
### 按键修饰符 ###
	
全部的按键别名：

- `.enter`
- `.tab`
- `.delete`（捕获“删除”和“退格”键）
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

可以通过全局`config.keyCodes`对象自定义按键修饰符别名

	Vue.config.keyCodes.aaa=13

#### 自动匹配按键修饰符 ####
可直接将`KeyboardEvent.key`暴露的任意有效按键名转换为kebab-case（短横线命名）来作为修饰符；

	<input @keyup.page-down="onPageDown"
在上面的例子中，处理函数仅在`$event.key === 'PageDown'`时被调用。

### 系统修饰键 ###
可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`

#### `.exact`修饰符 ####
2.5.0新增

`.exact`修饰符允许你控制由精确的系统修饰符组合触发的事件。

	<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
	<button @click.ctrl="onClick">A</button>
	
	<!-- 有且只有 Ctrl 被按下的时候才触发 -->
	<button @click.ctrl.exact="onCtrlClick">A</button>
	
	<!-- 没有任何系统修饰符被按下的时候才触发 -->
	<button @click.exact="onClick">A</button>

#### 鼠标按钮修饰符 ####
2.2.0新增

- `.left`
- `.right`
- `.middle`

## 表单输入绑定 ##
### 基础用法 ###
你可以用`v-model`指令在表单`<input>`及`<textarea>`元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

#### 文本 ####
	<input v-model="message" placeholder="edit me">
	<p>Message is: {{ message }}</p>

#### 多行文本 ####
	<span>Multiline message is:</span>
	<p style="white-space: pre-line;">{{ message }}</p>
	<br>
	<textarea v-model="message" placeholder="add multiple lines"></textarea>

#### 复选框 ####
单个复选框，绑定到布尔值

	<input type="checkbox" id="checkbox" v-model="checked">
	<label for="checkbox">{{ checked }}</label>

多个复选框，绑定到同一个数组：

	<div id='example-3'>
	  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
	  <label for="jack">Jack</label>
	  <input type="checkbox" id="john" value="John" v-model="checkedNames">
	  <label for="john">John</label>
	  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
	  <label for="mike">Mike</label>
	  <br>
	  <span>Checked names: {{ checkedNames }}</span>
	</div>

	new Vue({
	  el: '#example-3',
	  data: {
	    checkedNames: []
	  }
	})

#### 单选按钮 ####

	<div id="example-4">
	  <input type="radio" id="one" value="One" v-model="picked">
	  <label for="one">One</label>
	  <br>
	  <input type="radio" id="two" value="Two" v-model="picked">
	  <label for="two">Two</label>
	  <br>
	  <span>Picked: {{ picked }}</span>
	</div>

	new Vue({
	  el: '#example-4',
	  data: {
	    picked: ''
	  }
	})

#### 选择框 ####
单选时：

	<div id="example-5">
	  <select v-model="selected">
	    <option disabled value="">请选择</option>
	    <option>A</option>
	    <option>B</option>
	    <option>C</option>
	  </select>
	  <span>Selected: {{ selected }}</span>
	</div>

	new Vue({
	  el: '...',
	  data: {
	    selected: ''
	  }
	})

多选时（绑定到一个数组）：

	<div id="example-6">
	  <select v-model="selected" multiple style="width: 50px;">
	    <option>A</option>
	    <option>B</option>
	    <option>C</option>
	  </select>
	  <br>
	  <span>Selected: {{ selected }}</span>
	</div>
	
	new Vue({
	  el: '#example-6',
	  data: {
	    selected: []
	  }
	})

也可以用`v-for`渲染动态选项，具体的例子看官网。

### 值绑定 ###
对于单选按钮，复选框及选择框的选项，`v-model`绑定的值通常是静态字符串（对于复选框也可以是布尔值），但是有时我们可能把值绑定到Vue实例的一个动态属性上，这时可以用`v-bind`实现，并且这个属性的值可以不是字符串。

### 修饰符 ###
#### `.lazy` ####
在默认情况下，`v-model`在每次`input`事件触发后将输入框的值与数据进行同步（除了上述输入法组合文字时）。你可以添加`lazy`修饰符，从而转变为使用`change`事件进行同步。

#### `.number` ####
如果想自动将用户的输入值转为数值类型，可以给`v-model`添加`number`修饰符

#### `.trim` ####
如果要自动过滤用户输入的首尾空白符，可以给`v-model`添加`trim`修饰符。

## 组件基础 ##
### 组件的组织 ###
通常一个应用会以一棵嵌套的组件树的形式来组织，为了能在模板中使用，这些组件必须先注册以便Vue能够识别。这里有两种组件的注册类型：**全局注册**和**局部注册**。

全局注册的组件可以用在其被注册之后的任何（通过`new Vue`）新创建的Vue根实例，也包括其组件树中的所有子模板中。



## Vue基本概念 ##
- 全局api

	Vue.component  
	Vue.extend  
	Vue.filter等
- 选项

	**data**
	- 类型：`Object | Function`
	- 限制：组件的定义只接受`function`
	- 实例创建后，可以通过`vm.$data`访问原始对象

	**props** 
	- 类型：`Array<string> | Object`
	- 用于接收来自父组件的数据。对象的配置（type、default、required、validator）
 
	**computed** 
	- 类型：`{ [key: string]: Function | { get: Function, set: Function } }`  

	**methods** 
	- 类型：`{ [key: string]: Function }`
	 
	el  
	生命周期钩子等
- 实例属性/方法

	vm.$data  
	vm.$props  
	vm.$el  
	vm.$options  
	vm.$parent  
	vm.$root  
	vm.$children  
	vm.$slots（在使用渲染函数书写一个组件时，访问`vm.$slots`最优帮助）  
	vm.$watch  
	vm.$nextTick
- 指令

	参数：一些指令能够接收一个“参数”，在指令名称之后以冒号表示。
	- `v-html`：输出真正的html，而不是单纯的字符串
		
			<span v-html="rawHtml></span>
			data: {
				rawHtml: "<span style='color: red'>This should be red.</span>"
			}
			// 这个span的内容会被替换成reaHtml。
	- `v-once`：当数据改变时，插值处的内容不会更新。
	- `v-bind`
	- `v-on`：用于监听DOM事件，参数是监听的事件名。
	- `v-for`：`v-for`比`v-if`具有更高的优先级
	- `v-if`  
	- `v-show`
- 内置组件

	component  
	keep-alive  
	slot

参考学习资料：

- [vue中v-model和v-bind绑定数据的异同](https://www.tangshuang.net/3507.html)
