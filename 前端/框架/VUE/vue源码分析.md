# Vue源码 #
## 什么是AST ##
AST是指抽象语法树（abstract syntax tree），或者语法树（syntax tree），是源代码的抽象语法结构的树状表现形式。Vue在mount过程中，template会被编译成AST语法树。然后，经过generate（将AST语法树转化成render function字符串的过程）得到render函数，返回VNonde。VNode是Vue的虚拟DOM节点，里面包含标签名、子节点、文本等信息。

根据参数template属性生成render函数

	function Vue(options) {
		this._init(options);
	}

	Vue.prototype._init = function(options) {
		var vm = this;
		// 参数el属性存在，就调用mount方法；初始化时可以不传el属性，后续调用mount方法
		if(vm.$options.el) {
			vm.$mount(vm.$options.el);
		}
	}

	Vue.prototype.$mount = function() {
		var ref = compileToFunctions(template, {
			shouldDecodeNewlines: shouldDecodeNewlines,
			delimiters: options.delimiters,
			comments: options.comments
		}, this);
		// 由template参数得到render方法
		var render = ref.render;
		// 由template参数得到最大静态渲染树
		var staticRenderFns = ref.staticRenderFns;
	};

compileToFunctions生成render方法的具体实现

	var ast = parse(template.trim(), options);
	optimize(ast, options);
	var code = generate(ast, options);
首先根据template字符串生成ast对象，parse函数主要是通过正则表达式将str转换成树结构的对象。

然后对ast对象进行优化，找出ast对象中所有的最大静态子树（可以简单理解为不包含参数data属性的dom节点，每次data数据改变导致页面重新渲染的时候，最大静态子树不需要重新计算生成）。基本实现逻辑如下：

	function optimize(root, options) {
		// 将ast对象的所有节点标记为是否静态
		markStatic(root);
		markStaticRoots(root, false);
	}
	function markStatic(node) {
		// 当前节点是否静态
		node.static = isStatic(node);
		// 递归标记node的子节点是否静态
		for(var i = 0, l = node.children.length; i < l; i++) {
			var child = node.children[i];
			markStatic(child);
			// 只要有一个子节点非静态，父节点也非静态
			if(!child.static) {
				node.static = false;
			}
		}
	}

	function markStaticRoots(node, isInFor) {
		// 将包含至少一个非文本子节点（node.type === 3代表文本节点）的节点标记为最大静态树的根节点
		if(node.static && node.children.length && !(
			node.children.length === 1 &&
			node.children[0].type === 3
		)) {
			node.staticRoot = true;
			return
		} else {
			node.staticTRoot = false;
		}
		// 当前node节点不是静态节点，递归判断子节点
		if(node.children) {
			for(var i = 0, l = node.children.length; i < l; i++) {
				markStaticRoots(node.cildren[i], isInFOr || !!node.for);
			}
		}
	}

其中render是整个模板的渲染函数，staticrenderfns是静态树的渲染函数，staticrenderfns中的函数只会初始化一次，后续不需要再计算。

[Vue源码解析(一)-模版渲染](https://segmentfault.com/a/1190000011816036)