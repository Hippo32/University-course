# Symbol #
`Symbol`提供一种机制，保证每个属性的名字都是独一无二的，从根本上防止属性名的冲突。

ES6引入了一种新的原始数据类型`Symbol`，表示独一无二的值。

`Symbol`值通过Symbol函数生成。这就是说，对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的Symbol类型。凡是属性名属于Symbol类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。

`Symbol`函数可以接受一个字符串作为参数，表示对Symbol实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。如果参数是一个对象，就会调用该对象的`toString`方法，将其转为字符串，然后才生成一个Symbol值。

注意：

- Symbol函数前不能使用`new`命令。

## Symbol的转换 ##
### Symbol值可以显式转为字符串 ###
	var sym = Symbol('hello');
	String(sym) // 'Symbol(hello)'
	sym.toString() // 'Symbol(hello)'
### Symbol值转布尔值 ###
	Boolean(sym) // true
	!sym // false
	if (sym) {}
### Symbol不能转为数值 ###

## Symbol值作为对象属性名 ##
可以用方括号和`Object.defineProperty`，不能用点运算符。

## 方法 ##
### Symbol.for(key) ###
根据给定的键key，来从运行时的symbol注册表中找到对应的symbol，如果找到了，则返回它，否则，新建一个与该键关联的symbol，并放入全局symbol注册表中。

参数：
 
- key：一个字符串，作为symbol注册表中与某symbol关联的键（同时也会作为该symbol的描述）。

返回值：返回由给定的key找到的symbol，否则就是返回新创建的symbol。

### Symbol.keyFor(sym) ###
用来获取symbol注册表中与某个symbol关联的键。

	var s1 = Symbol.for('foo')
	Symbol.keyFor(s1)
	"foo"

需要注意的是，`Symbol.for`为Symbol值登记的名字，是全局环境的，可以在不同的iframe或service worker中取到同一个值。