# JavaScript的对象 #
## ES6的对象简写 ##
	const Person = {
		name: 'xiaoming',
		birth, // 等同于birth: birth
		hello() { console.log('my name is', this.name); } // 等同于hello: function() ...
	};
## ES6允许在字面量定义对象时，用表达式作为属性名 ##
	let obj = {
		['a' + 'bc']: 123
	};
> 注意：属性名表示法与简洁表示法，不能同时使用，会报错。  
> 属性名表达式如果是一个对象，默认情况下会自动将对象转为字符串`[object Object]`。

## 方法 ##
### Object.getOwnPropertyDescriptor(obj, prop) ###
返回指定对象上一个自由属性对应的属性描述符（自有属性指的是直接赋予该对象的属性，不需要从原型链上进行查找的属性）。

参数：

- obj：需要查找的目标对象
- prop：目标对象内属性名称（String类型）

返回值：如果指定的属性存在于对象上，则返回其属性描述符（property descriptor），否则返回`undefined`。

### Object.getOwnPropertyDescriptors ###
返回指定对象所有自身属性（非继承属性）的描述对象。

用处：

- 配合`Object.create`方法，将对象属性克隆到一个新对象。这属于浅拷贝。
- 实现一个对象继承另一个对象。
- 实现Mixin（混入）模式。

### Object.defineProperty(obj, prop, descriptor) ###
会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回这个对象。

参数：

- obj：要在其上定义属性的对象。
- prop：要定义或修改的属性的名称。
- descriptor：将被定义或修改的属性描述符。

返回值：被传递给函数的对象。

	Object.defineProperty(obj, "key", {
		enumerable: false,
		configurable: false,
		writable: false,
		value: "static"
	});

### Object.defineProperties(obj, props) ###
直接在一个对象上定义新的属性或修改现有属性，并返回该对象。

参数：

- obj：在其上定义或修改属性的对象。
- props：要定义其可枚举属性或修改的属性描述符的对象。
	- configurable：`true`当且仅当该描述符的类型可以被改变并且该属性可以从对应对象中删除。默认为`false`。
	- enumerable：`true`当且仅当在枚举相应对象上的属性时该属性显现。默认为`false`。
	- value：与属性关联的值。可以是任何有效的JavaScript值（数字，对象，函数等）。默认为`undefined`。
	- writable
	- get
	- set

返回值：传递给函数的对象。

	var obj = {};
	Object.defineProperties(obj, {
		'property1': {
			value: true,
			writable: true
		},
		'property2': {
			value: 'Hello',
			writable: false
		}
	});
	
### Object.is(value1, value2) ###
判断两个值是否是相同的值。`-0`不等于`+0`，`NaN`等于`NaN`。不会做默认类型转换。

参数：

- value1：需要比较的第一个值
- value2：需要比较的第二个值

返回值：表示两个参数是否相同的Boolean。

### Object.keys(obj) ###
返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和使用`for...in`循环遍历该对象时返回的顺序一致。（返回的是key）

参数：

- obj：要返回其枚举自身属性的对象。

返回值：一个表示给定对象的所有可枚举属性的字符串数组。

### Object.values(obj) ###
返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值。（返回的是value）

参数： 

- obj：被返回可枚举属性值的对象。

返回值：一个包含对象自身的所有可枚举属性值的数组。

注意：

- `Object.values`会过滤属性名为Symbol值的属性。
- 如果参数是一个字符串，会返回各个字符组成的一个数组。

### Object.entries(obj) ###
返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组。（返回键值对）

参数：

- obj：可以返回其可枚举属性的键值对的对象。

返回值：给定对象自身可枚举属性的键值对数组。

注意：

- 如果原对象的属性名是一个Symbol值，该属性会被忽略。

### Object.assign(target, ...sources) ###
用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。

浅拷贝。

参数：

- target：目标对象
- sources：源对象

返回值：目标对象

注意：

- 同名属性会被覆盖。
- 只有一个参数，会直接返回该参数。
- 如果参数不是对象，则会先转成对象，然后返回。
- `undefined`和`null`不能作为首参数，会报错。
- 数值、字符串和布尔值不在首参数，也不会报错。但是，除了字符串会以数组形式，拷贝入目标对象，其他值都不会产生效果。
- 只拷贝对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（`enumerable: false`）。
- 可以用来处理数组，但是会把数组视为对象。
- 只能进行值的复制，如果复制的值是一个取值函数，那么将求值后再复制。

用途：

- 为对象添加属性
- 为对象添加方法
- 克隆对象（只能克隆原始对象自身的值，不能克隆它继承的值）
- 合并多个对象
- 为属性指定默认值

### Object.create() ###
`Object.create`方法的第二个参数添加的对象属性，如果不显式声明，默认是不可百年里的。

### Object.getPrototypeOf(object) ###
返回指定对象的原型（内部`[[Prototype]]`属性的值）。

参数：

- obj：要返回其原型的对象。

返回值：给定对象的原型。如果没有继承属性，则返回null。

注意：

- 如果参数不是对象，会被自动转为对象。

### Object.setPrototypeOf(obj, prototype) ###
设置一个指定的对象的原型（即，内部[[Prototype]]属性）到另一个对象或null。

> 注意：会很慢，尽量避免，建议用`Object.create()`。

参数：

- obj：要设置其原型的对象。
- prototype：该对象的新原型（一个对象或null）。

返回值：返回参数对象本身。


## 属性的可枚举性enumerable ##
如果该属性为`false`，就表示某些操作会忽略当前属性。

目前，有四个操作会忽略`enumerable`为`false`的属性：

- `for...in`循环：只遍历对象自身和继承的可枚举的属性。
- `Object.keys()`：返回对象自身的所有可枚举的属性的键名。
- `JSON.stringify()`：只串行化对象自身的可枚举的属性。
- `Object.assign()`：只拷贝对象自身的可枚举的属性。

只有`for...in`会返回继承的属性 ，其他三个方法都会忽略继承的属性，只处理对象自身的属性。

## 属性的遍历 ##
ES6一共有5种方法可以便利对象的属性。

1. `for...in`：循环遍历对象自身和可继承的可枚举属性（不含Symbol属性）。
2. `Object.keys(obj)`：返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含Symbol属性）的键名。
3. `Object.getOwnPropertyNames(obj)`：返回一个数组，包括对象自身的所有属性（不含Symbol属性，但是包括不可枚举属性）的键名。
4. `Object.getOwnPropertySymbols(obj)`：返回一个数组，包含对象自身的所有Symbol属性的键名。
5. `Reflect.ownKeys(obj)`：返回一个数组，包含对象自身的所有键名，不管键名时Symbol或字符串，也不管是否可枚举。

属性遍历的次序规则：

- 首先遍历所有数值键，按照数值升序排列。
- 其次遍历所有字符串，按照加入时间升序排列。
- 最后遍历所有Symbol键，按照加入时间升序排排列。

## super关键字 ##
`this`关键字总是指向函数所在的当前对象。

`super`指向当前对象的原型对象。

`super`关键字表示原型对象时，只能用在对象的方法之中，用在其他地方都会报错。目前，只有对象方法的简写法可以让JavaScript引擎确认，定义的是对象的方法。

## 对象的扩展运算符 ##
对象的扩展运算符（`...`）用于取出参数对象的所有可遍历属性，拷贝到当前对象之中。
### 解构赋值 ###
	var {x, y, ...z} = {x:1, y: 2, a: 3, b: 4}

- 解构赋值要求等号右边是一个对象。
- 解构赋值必须是最后一个参数。
- 解构赋值的拷贝是浅拷贝。
- 扩展运算符的解构赋值，不能复制继承自原型对象的属性。
- 变量声明语句之中，如果使用解构赋值，扩展运算符后面必须是一个变量名，而不是是一个解构赋值表达式。

## 属性 ##
### __proto__属性 ###
用来读取或设置当前对象的`prototype`对象。