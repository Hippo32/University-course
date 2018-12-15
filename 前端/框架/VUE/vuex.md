# Vuex #
> 改变**store**中的状态的唯一途径就是显式地**提交mutation**

## 创建简单的Store ##
	const store = new Vuex.Store({
		state: {
			count: 0
		},
		mutations: {
			increment(state) {
				state.count++
			}
		}
	})
通过`store.state`来获取状态对象  
通过`store.commit`方法触发状态变更

	store.commit('increment')

	console.log(store.state.count)