# Vue Router #
## 常规操作 ##
1. 定义（路由）组件。可以从其他文件 import 进来。

		const Foo = { template: '<div>foo</div>' }
		const Bar = { template: 'div> bar</div>' }
2. 定义路由

	每个路由应该应该映射一个组件。其中“component”可以是通过`Vue.extend()`创建的组件构造器，或者，只是一个组件配置对象。

		const routes = [
			{ path: '/foo', component: Foo },
			{ path: '/bar', component: Bar }
		]
3. 创建router实例，然后传 routes 配置。

		const router = new VueRouter(P
			routes
		})
4. 创建和挂载根实例。

		const app = new Vue({
			router
		}).$mount('#app')

可以在任何组件内通过`this.$router`访问路由器，也可以通过`this.$route`访问当前路由。

完整例子：

	<div id="app">
	  <p>
	    <router-link to="/user/foo">/user/foo</router-link>
	    <router-link to="/user/bar">/user/bar</router-link>
	  </p>
	  <router-view></router-view>
	</div>
	
	// js
	const User = {
	  template: `<div>User {{ $route.params.id }}</div>`
	}	
	const router = new VueRouter({
	  routes: [
	    { path: '/user/:id', component: User }
	  ]
	})	
	const app = new Vue({ router }).$mount('#app')

## 动态路由匹配 ##
有时，同一个路径可以匹配多个路由，此时，匹配的优先级就按照路由的定义顺序：谁先定义的，谁的优先级就最高。

## 嵌套路由 ##
可以在template中嵌套`<router-view>`。要在嵌套的出口中渲染组件，需要在`VueRouter`的参数中使用`children`配置。

例：

	const router = new VueRouter({
		routes: [
			{ path: '/user/:id', component: User,
			  children: [
				{ path: 'profile', component: UserProfile },
				{ path: 'posts', component: UserPosts }
			  ]
			}
		]
	})

**要注意，以`/`开头的嵌套路径会被当作根路径。这让你充分的使用嵌套组件而无须设置嵌套的路径。**


参考：

- [官网api](https://router.vuejs.org/zh/api/)