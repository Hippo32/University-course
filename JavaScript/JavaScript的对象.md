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

### Object.is(value1, value2) ###
判断两个值是否是相同的值。`-0`不等于`+0`，`NaN`等于`NaN`。不会做默认类型转换。

参数：

- value1：需要比较的第一个值
- value2：需要比较的第二个值

返回值：表示两个参数是否相同的Boolean。

### Object.assign(target, ...sources) ###
用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。

参数：

- target：目标对象
- sources：源对象

返回值：目标对象

注意：

- 同名属性会被覆盖。
- 只有一个参数，会直接返回该参数。
- 如果参数不是对象，则会先转成对象，然后返回。
- `undefined`和`null`不能作为首参数，会报错。
- 数值、字符串和布尔值不在首参数
