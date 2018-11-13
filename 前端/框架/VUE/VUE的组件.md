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

### Vue.extend() ###
用法：使用基础Vue构造器，创建一个“子类”。参数是一个包含组件选项的对象。`data`选项是特例，需要注意：在`Vue.extend()`中它必须是函数。

	<div id="mount-point"></div>

	// 创建构造器
	var Profile = Vue.extend({
		template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
		data: function() {
			return {
			firstName: 'Walter',
			lastName: 'White',
			alias: 'Heisenberg'
			}
		}
	})
	//创建Profile实例，并挂载到一个元素上
	new Profile().$mount('#mount-point')
	

## 组件里的数据传递 ##
### 用prop向子组件传递数据 ###
prop可以让你在组件上注册一些自定义特性。当一个值传递给自定义的那个prop特性的时候，它就变成了那个组件实例的一个属性。

	Vue.component('blog-post', {
		props: ['title'],
		template: '<h3>{{ title }}</h3>'
	})

	<blog-post title="My journey with Vue"></blog-post> //可以用v-bind动态传递prop

> HTML中的特姓名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。如果props在Vue.component中采用的是camelCase（驼峰命名法），则在HTML中要用kebab-case（短横线分隔命名）命名。

	Vue.component('blog-post', {
		props: ['postTitle'],
		template: '<h3>{{ postTitle }}</h3>'
	})
	
	<blog-post post-title="hello"></blog-post>
>如果你使用字符串模板，那么这个限制就不存在了。

**prop类型**

- 字符串数组形式
- 对象形式：key：属性名称，value：类型

		props: ['title', 'likes']
		props: {
			title: String,
			likes: Number
		}

**传递prop**

根据测试，静态传prop，不管传什么类型，都会被vue当成字符串。所以如果要传除了字符串的其它类型，都要用`v-bind`

静态传prop：`value="23"`

动态传prop（通过`v-bind`）：`:value="checked"`，Vue实例的data那里要声明了`checked`。

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

**单向数据流**

prop传值属于单向传值，只能由父级传向子组件。父级prop的更新会向下流动到子组件中，但是反过来则不行。不应该在一个子组件内部改变prop。如果你这样做了，Vue会在浏览器的控制台中发出警告。

这里有两种常见的试图改变一个prop的情形：

1. 这个prop用来传递一个初始值；这个子组件接下来希望将其作为一个本地的prop数据来使用。在这种情况下，最好定义一个本地的data属性并将这个prop用作其初始值：

		props: ['initialCounter'],
		data: function() {
			return {
				counter: this.initialCOunter
			}
		}
2. 这个prop以一种原始的值传入且需要进行转换。在这种情况下，最好使用这个prop的值来定义一个计算属性：

		props: ['size'],
		computed: {
			normalizedSize: function() {
				return this.size.trim().toLowerCase()
			}
		}

**prop验证**

	Vue.component('my-component', {
		props: {
			// 基础的类型检查（'null'匹配任何类型）
			propA: Number,
			// 多个可能的类型
			propB: [String, Number],
			//必填的字符串
			propC: {
				type: String,
				required: true
			},
			// 带有默认值的数字
			propD: {
				type: Number,
				default: 100
			},
			// 带有默认值的对象
			propE: {
				type: Object,
				// 对象或数组默认值必须从一个工厂函数获取
				default: function () {	
					retrurn { message: 'hello' }
				}
			},
			// 自定义验证函数
			propF: {
				validator: function (value) {
					// 这个值必须匹配下列字符串中的一个
					return ['success', 'warning', 'danger'].indexOf(value) !== -1
				}
			}
		}
	})

自定义验证函数的例子

    <div id="app">
            <test :author="post"></test>
    </div>
    <script>
        Vue.component('test', {
            props: {
                author: {
                    validator: function (value) {
                        return ['success', 'warning', 'danger'].indexOf(value) !== -1
                    }
                }      
            },
            template: "<p>{{author}}</p>",      
        })          
        var vm = new Vue({
            el: "#app",
            data: {
                post: "warning"
            }
        })
    </script>
`type`可以是下面原生构造器：String、Number、Boolean、Function、Object、Array、Symbol。`type`还可以是一个自定义的构造函数，并且通过`instanceof`来进行检查确认。

	// 自定义构造函数的例子
    <div id="app">
            <test :author="post"></test>
    </div>
    <script>
        Vue.component('test', {
            props: {
                author: Person
                },
            template: "<p>{{test}}</p>", // true
            computed: {
                test: function() {
                    return this.author instanceof Person;
                }
            }
        })
        function Person(first, last) {
            this.first = first;
            this.last = last;
        } 
        var vm = new Vue({
            el: "#app",
            data: {
                post: new Person("aaa", "bbb")
            }
        })
    </script>

**非Prop的特性**

非Prop特性是指可以添加任意的特性在父级，而且这些特性会被添加到这个组件的根元素上，但是父级不能向子组件传递这个特性的信息。

**哪些特性会被替换，哪些特性会被合并？**  
对于绝大多数特性来说，从外部提供给组件的值会替换组件内部设置好的值。所以如果传入`type="text"`就会替换掉`type="date"`并把它破坏！庆幸的是，`class`和`style`特性会稍微智能一下，即两边的值会被合并起来，从而得到最终的值。

如果不希望组件的根元素继承特性，可以在组件的选项中设置`inheritAttrs: false`。但是`class`特性还是会继承。

----------


### 通过事件向父级组件发送消息 ###
在子组件中用`$emit`方法传入事件的名字，来向父级事件触发一个事件。第一个参数是事件的名字，第二个参数可以是要传给父组件的参数。

	<button v-on:click="$emit('enlarge-text', 0.1)">...

父级可以用`v-on`监听这个事件。可以用`$event`访问到子组件传过来的值。如果这个事件处理函数是一个函数，则这个值会作为第一个参数传入这个参数。

也可以用`this.$emit('enlarge-text')`来触发。

推荐始终使用kebab-case的事件名。


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

一个组件的`v-model`默认会利用名为`value`的prop名和名为`input`的事件，但是像单选框、复选框等类型的输入控件可能会将`value`特性用于不同的目的。`model`选项可以用来避免这样的冲突。

	Vue.component('base-checkbox', {
		model: {
			prop: 'checked',
			event: 'change'
		}, 
		props: {
			checked: Boolean
		},
		template: `
			<input
				type="checkbox"
				:checked="checked"
				@change="$emit('change', $event.target.checked)"
			>
		`
	})
这个时候在这个组件上使用`v-model`，用的是名为`checked`的prop名和名为`change`的事件。

注意：你仍然需要在组件的`props`选项里声明`checked`这个prop。

### 将原生事件绑定到组件 ###
`$listeners`：包含可父作用域中的（不含`.native`修饰器的）`v-on`事件监听器。它可以通过`v-on="$listeners"`传入内部组件。

### .sync修饰符 ###
	this.$emit('update: title', newTitle)

	// 父组件可以监听这个事件，并且（如果需要的话）更新一个本地数据属性。例如：
	<text-document
		v-bind:title="doc.title"
		v-on:update:title="doc.title = $event"
	></text-document>
为了简便，我们使用`.sync`修饰符，提供了这个模式的简写：

	<text-document v-bind:title.sync="doc.title"></text-document>

## 插槽 ##

## 动态组件 ##
用`component`标签和`is`实现。

	<component v-bind:is="xxx"></component>

## 处理边界情况 ##
- 访问根实例：`$root`
- 访问父级组件实例：`$parent`
- 访问子组件实例或子元素：通过`ref`特性为子组件赋予一个ID引用，用`$refs`来访问。
- 依赖注入：
	- `provide`：允许我们指定我们想要提供给后代组件的数据/方法。

			provide: function() {
				return {
					getMap: this.getMap
				}
			}
	- `inject`：接收指定的我们想要添加在这个实例上的属性。

			inject: ['getMap']

- 程序化的事件侦听器
	- 通过`$on(eventName, eventHandler)`侦听一个事件
	- 通过`$once(eventName, eventHandler)`一次性侦听一个事件
	- 通过`$off(eventName, eventHandler)`停止侦听一个事件


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