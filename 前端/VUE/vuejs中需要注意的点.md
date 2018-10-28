# vuejs中需要注意的点 #

----------

camelCase vs. kebab-case
-

HTML 特性是不区分大小写的。所以，当使用的不是字符串模板时，camelCase (驼峰式命名) 的 prop 需要转换为相对应的 kebab-case (短横线分隔式命名)：

	Vue.component('child', {
  		// 在 JavaScript 中使用 camelCase
  		props: ['myMessage'],
  		template: '<span>{{ myMessage }}</span>'
	})


	<!-- 在 HTML 中使用 kebab-case -->
	<child my-message="hello!"></child>

单向数据流
-
Prop是单向绑定的：当父组件的属性变化时，将传导给子组件，但是反过来不会。

另外，每次父组件更新时，子组件的所有prop都会更新为最新值。这意味着你不应该在子组件内部改变prop。如果你这么做了，Vue会在控制台给出警告。

子组件向父组件传值的问题：[https://www.cnblogs.com/xxcanghai/p/6124699.html?_t=t](https://www.cnblogs.com/xxcanghai/p/6124699.html?_t=t)

## vue的组件中data要避免引用赋值 ##
在new一个vue实例时，data的写法如下：
	
	new Vue({
		data: {
			fruit: 'apple'
		}
	})
而在一个vue组件中，要避免引用赋值，即改变一个组件实例中data里的值，其他组件实例中data的值也随之改变，这样显然不是我们想要的，所以要写成函数的形式：
	
	data: function()  {
		return {
			fruit: 'apple'
		}
	}

## 关于:class ##
2018/10/27 22:13:13 

`:class=`后面跟什么，什么时候需要加`""`，加了有什么效果，不加又会有什么效果。

方案一：
	
	// HTML代码
	<button v-for="(tab, index) in tabs"
			:key="index"
			:class=tab>{{ tab }}
	</button>

	// JS代码
	data: { tabs: ["tab1", "tab2"] }

	// 渲染后
	<button class="tab1">tab1</button>
	<button class="tab2">tab2</button>

方案二：

	// HTML代码，把:class地方变一下，JS不变，下同
	:class="tab"

	// 渲染后
	<button class="tab1">tab1</button>
	<button class="tab2">tab2</button>

方案三：

	// HTML代码
	:class=["tab"]
	// 或
	:class="['tab']" // 一般都需要在外面包裹一层""

	// 渲染后
	<button class="tab">tab1</button>
	<button class="tab">tab2</button>	

方案四：

	// HTML代码
	:class="[tab]"

	// 渲染后
	<button class="tab1">tab1</button>
	<button class="tab2">tab2</button>

结论：

`:class=`后面通常需要包裹一层`""`，写在`""`里的需要在data中有声明的，但是写在`[]`里的用`""`包裹的则表示的是字符串，不需要在data中声明。

## v-if和v-show ##
2018/10/28 11:59:59 

如果`v-if`初始渲染时条件为假，则不渲染里面的代码，所以即使里面的代码有错也不会报错。而`v-show`始终会渲染里面的代码，只是基于CSS进行简单的切换，所以里面的代码也会报错。

## 用v-for给其中的一个单独设置类名 ##
2018/10/28 23:00:42 

[源码参考](https://github.com/Hippo32/DIST/blob/master/distcode/vue/%E7%BB%99%E5%88%97%E8%A1%A8%E5%8D%95%E7%8B%AC%E6%B7%BB%E5%8A%A0%E4%B8%80%E4%B8%AA%E7%B1%BB%E5%90%8D.html)
	