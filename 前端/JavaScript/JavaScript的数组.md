# JavaScript的数组 #
## 方法 ##
### Array.prototype.slice() ###
`slice()`方法返回一个从开始到结束（不包括结束）选择的数组的一部分浅拷贝到一个新数组对象。且原始数组不会被修改。

返回值：一个含有提取元素的新数组。

`slice`方法可以用来将一个类数组对象/集合转换成一个新数组。

	function list() {
		return Array.prototype.slice.call(arguments);
	}
	var list1 = list(1, 2, 3); // [1, 2, 3]

除了使用`Array.prototype.slice.call(arguments)`，还可以使用`[].slice.call(arguments)`来代替。另外，你可以使用`bind`来简化该过程。

	var unboundSlice = Array.prototype.slice;
	var slice = Function.prototype.call.bind(unboundSlice);

	function list() {
		return slice(arguments);
	}

	var list1 = list(1, 2, 3); // [1, 2, 3]
### Array.from() ###
`Array.from`方法用于将两类对象转为真正的数组：类似数组的对象和可遍历的对象（包括es6新增的数据结构Set和Map）。

只要是部署了Iterator接口的数据结构，`Array.from`都能将其转为数组。

如果参数是一个真正的数组，`Array.from`会返回一个一模一样的新数组。

`Array.from`还可以接受第二个参数，作用类似于数组的`map`方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

如果`map`函数里面用到了`this`关键字，还可以传入`Array.from`的第三个参数，用来绑定`this`。

`Array.from()`可以将各种值转为真正的数组，并且还提供`map`功能。这实际上意味着，只要有一个原始的数据结构，你就可以先对它的值进行处理，然后转成规范的数组结构，进而就可以使用数量众多的数组方法。

### Array.of() ###
`Array.of()`方法用于将一组值，转换为数组。

	Array.of(3, 11, 8)
`Array.of`基本上可以用来替代`Array()`或`new Array()`，并且不存在由于参数不同而导致的重载。

`Array.of`总是返回参数值组成的数组。如果没有参数，就返回一个空数组。

`Array.of`方法可以用下面的代码模拟实现

	function ArrayOf() {
		return [].slice.call(arguments);
	}

### copyWithin() ###
`copyWithin`方法，在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，会修改当前数组。

	Array.prototype.copyWithin(target, start = 0, end = this.length)
它接受三个参数。

- target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
- start（可选）：从该位置开始读取数据，默认为0。如果为负值，表示倒数。
- end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。

		[1,2,3,4,5].copyWithin(0,3)
		// (5) [4, 5, 3, 4, 5]
		// 从3号位知道数组结束的成员（4和5），复制到从0号位开始的位置，结果覆盖了原来的1和2。

### find() ###
`find()`方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，知道找出第一个返回值为`true`的成员，然后返回该成员。如果没有符合条件的成员，则返回`undefined`。

可以接受第二个参数，用来绑定回调函数的`this`对象。

`find`方法的回调函数可以接受三个参数，一次为当前的值、当前的位置和原数组。



### findIndex ###
与`find`方法非常类似，返回第一个符合条件的数组成员的位置。如果所有成员都不符合，则返回`-1`。

可以接受第二个参数，用来绑定回调函数的`this`对象。

可以发现`NaN`，弥补了数组的`indexOf`方法的不足。

	[NaN].indexOf(NaN)
	// -1

	[NaN].findIndex(y => Object.is(NaN, y))
	// 0

### fill() ###
`fill`方法使用给定值，填充一个数组。

`fill`方法用于空数组的初始化非常方便。数组中已有的元素，会被全部抹去。

`fill`方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束为止。

注意，如果填充的类型为对象，那么被赋值的是同一个内存地址的对象，而不是深拷贝对象。

### entries()、keys()和values() ###
用于遍历数组。它们都返回一个遍历器对象，可以用`for...of`循环进行遍历，唯一的区别是`keys()`是对键名的遍历、`values()`是对键值的遍历，`entries()`是对键值对的遍历。

### includes() ###
`Array.prototype.includes`方法返回一个布尔值，表示某个数组是否包含给定的值，与字符的`includes`方法类似。

该方法的第二个参数表示搜索的起始位置，默认为0.如果第二个参数为负数，则表示倒数的位置，如果这是它大于数组长度（比如第二个参数为-4，但数组长度为3），则会重置为从0开始。

`indexOf`方法有两个缺点，一是不够语义化，它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于`-1`，表达起来不够直观。二是，它内部使用严格相等运算符（`===`)进行判断，这会导致对`NaN`的误判。

### flat()、flatMap() ###
数组的成员有时还是数组，`Array.prototype.flat()`用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响。

`flat()`默认只会“拉平”一层，如果想要“拉平”多层的嵌套数组，可以将`flat()`方法的参数写成一个整数，表示想要拉平的层数，默认为1。

如果不管有多少层嵌套，都要转成一维数组，可以用`Infinity`关键字作为参数。

如果原数组有空位，`flat()`方法会跳过空位。

`flatMap()`方法对原数组的每个成员执行一个函数（相当于执行`Array.prototype.map()`），然后对返回值组成的数组执行`flat()`方法。该方法返回一个新数组，不改变原数组。

`flatMap()`只能展开一层数组。

`flatMap()`方法的参数是一个遍历函数，该函数可以接受三个参数，分别是当前数组成员、当前数组成员的位置（从零开始）、原数组。

`flatMap()`方法还可以有第二个参数，用来绑定遍历函数里面的this。
## 扩展运算符 ##
扩展运算符是三个点（`...`）。该运算符将一个数组，变为参数序列。

	console.log(...[1, 2, 3])
	// 1 2 3

用法：

- 可以取代`apply`方法，将数组作为函数的参数。

		f(...[1,2,3]);
		arr1.push(...arr2);
- 复制数组

		const a1 = [1, 2];
		// 写法一
		const a2 = [...a1];
		// 写法二
		const [...a2] = a1;
- 合并数组（都是浅拷贝，要注意）

		// es5
		arr1.concat(arr2, arr3);
		// es6
		[...arr1, ...arr2, ...arr3]
- 解构赋值

		var [first, ...rest] = [1, 2, 3, 4, 5];
		first //1
		rest // [2, 3, 4, 5]
	如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。
- 将字符串转为真正的数组。能够正确识别四个字节的Unicode字符。
	
		[...'hello']
		// ["h", "e", "l", "l", "o"]

		// 正确返回字符串长度的函数
		function length(str) {
			return [...str].length;
		}
	凡是涉及到操作四个字节的Unicode字符的函数，最好都用扩展运算符改写。
- 只要具有Iterator接口的对象，都可以使用扩展运算符，Map和Set结构，Generator函数

## 空位 ##
ES5对空位的处理，已经很不一致了，大多数情况下会忽略空位。

- `forEach()`，`filter()`，`reduce()`，`every()`和`some()`都会跳过空位。
- `map()`会跳过空位，但会保留这个值
- `join()`和`toString()`会将空位视为`undefined`，而`undefined`和`null`会被处理成空字符串。

ES6则是明确将空位转为`undefined`

## 数组解构 ##
	let arr1 = [1, 3];
	let arr2 = [4, 8];

	// 方法一
	arr1.concat(arr2)

	// 方法二
	[...arr1, ...arr2]


