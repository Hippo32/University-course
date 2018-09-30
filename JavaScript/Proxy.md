# Proxy #
Proxy用于修改某些操作的默认行为。

ES6原生提供Proxy构造函数，用来生成Proxy实例。

	var proxy = new Proxy(target, handler)

参数：

- target：表示所要拦截的目标对象。即如果没有Proxy的介入，操作原来要访问的就是这个对象。
- handler：一个配置对象，用来定制拦截行为。对于每一个被代理的操作，需要提供一个对应的处理函数，该函数将拦截对应的操作。

例子：

	var proxy = new Proxy({}, {
		get: function(target, property) {
			return 32;
		}
	});
	proxy.time  // 32
	proxy.name  // 32

## 用法 ##
- Proxy实例可以作为其他对象的原型对象。
- 同一个拦截器函数，可以设置多个操作。

## Proxy支持的拦截操作 ##
- `get(target, propKey, receiver)`：拦截对象属性的读取，比如`proxy.foo`和`proxy['foo']。
	- 三个参数，依次为目标对象、属性名和proxy实例本身（严格地说，是操作行为所针对的对象），其中最后一个参数可选。
	- `get`方法可继承。
	- 如果一个属性不可配置（configurable）且不可写（writable），则Proxy不能修改该属性。
- `set(target, propKey, value, receiver)`：拦截对象属性的设置，比如`proxy.foo = v`或`proxy['foo'] = v`，返回一个布尔值。
	- 可以接受四个参数：依次为目标对象、属性名、属性值和Proxy实例本身，其中最后一个参数可选。
	- 利用`set`方法，还可以数据绑定，即每当对象发生变化时，会自动更新DOM。
	- 如果木匾对象自身的某个属性，不可写且不可配置，那么set方法将不起作用。
- `has(target, propKey)`：拦截`propKey in proxy)`的操作，返回一个布尔值。
	- 接受两个参数，分别是目标对象、需查询的属性名。
	- 如果原对象不可配置或者禁止扩展，这时`has`拦截会报错。
	- has拦截对`for...in`循环不生效。
- `deleteProperty(target, propKey)`：拦截`delete proxy[propKey]`的操作，返回一个布尔值。
	- 如果这个方法抛出错误或者返回false，当前属性就无法被delete命令删除。
	- 目标对象自身的不可配置的属性，不能被deleteProperty方法删除，否则报错。
- `ownKeys(target)`：拦截`Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`、`for...in`循环，返回一个数组。该方法返回目标对象所有自身的属性名，而`Object.keys()`的返回结果仅包括目标对象自身的可遍历属性。
	- `ownKeys`方法返回的数组成员，只能是字符串或 Symbol 值。如果有其他类型的值，或者返回的根本不是数组，就会报错。
- `getOwnPropertyDescriptor(target, propKey)`：拦截`Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象。
- `defineProperty(target, propKey, propDesc)`：拦截`Object,defineProperty(proxy, propKey, propDesc)`、`Object.defineProperties(proxy, propDesc)`，返回一个布尔值。
- `preventExtensions(target)`：拦截`Object.preventExtensions(proxy)`，返回一个布尔值。
- `getPrototypeOf(target)`：拦截`Object.getPrototypeOf(proxy)`，返回一个对象。
	- 拦截下面这些操作：
		- `Object.prototype.__proto__`
		- `Object.prototype.isPrototypeOf()`
		- `Object.getPrototypeOf()`
		- `Reflect.getPrototypeOf()`
		- `instanceof`
	- 返回值必须是对象或者null，否则报错。
- `isExtensible(target)`：拦截`Object.isExtensible(proxy)`，返回一个布尔值。
- `setPrototypeOf(target, proto)`：拦截`Object.setPrototypeOf(proxy, proto)`，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。
- `apply(target, object, args)`：拦截 Proxy 实例作为函数调用的操作，比如`proxy(...args)`、`proxy.call(object, ...args)`、`proxy.apply(...)`。
	- 接受三个参数，分别是目标对象、目标对象的上下文对象（this）和目标对象的参数数组。
- `construct(target, args)`：拦截 Proxy 实例作为构造函数调用的操作，比如`new proxy(...args)`。
	- 接受两个参数：`target`：目标对象。`args`：构造函数的参数对象。
	- 返回的必须是一个对象，否则会报错。