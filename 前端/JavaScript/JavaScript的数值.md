# JavaScript的数值 #

## 二进制和八进制表示法 ##
`0b` —— 二进制

`0o` —— 八进制

`0x`或`0X`——十六进制

通常来说，有前导0的数值会被视为八进制，但是如果前导0后面有数字8和9，则该数值被视为十进制。

如果要将`0b`和`0o`前缀的字符串数值转为十进制，要使用`Number`方法。
	
	Number('0b111') // 7

## 科学计数法 ##
科学计数法允许字母`e`或`E`的后面，跟着一个整数，表示这个数值的指数部分。

	123e3 // 123000
	123e-3 // 0.123
	-3.1E+12 // -3100000000000
	.1e-23 // 1e-24

以下两种情况，JavaScript会自动将数值转为科学计数法表示，其他情况都采用字面形式直接表示。

1. 小数点前的数字多于21位。
2. 小数点后的零多于5个。

		0.0000003 // 3e-7

## 正零和负零 ##
	-0 === +0 // true
	0 === -0 // true
	0 === +0 // true
几乎所有场合，正零和负零都会被当作正常的0。

唯一有区别的场合是，`+0`或`-0`当做分母，返回的值是不相等的。

	(1 / +0) === (1 / -0) // false
上面的代码之所以出现这样的结果，是因为除以正零得到`+Infinity`，除以负零得到`-Infinity`，这两者是不相等的。

## NaN ##
NaN是JavaScript的特殊值，表示“非数字”，主要出现在将字符串解析成数字出错的场合。

1. NaN不等于任何值，包括它本身。
2. 数组的`indexOf`方法内部使用的是严格相等运算符，所有该方法对`NaN`不成立。
3. `NaN`在布尔运算时被当作`false`。
4. `NaN`与任何数（包括它自己）的运算，得到的都是`NaN`。

## Infinity ##
Infinity表示“无穷”，用来表示两种场景。一种是一个正的数值太大，或一个负的数值太小，无法表示；另一种是非0数值除以0，得到Infinity。

	// 场景一
	Math.pow(2, 1024)
	// Infinity

	// 场景二
	0 / 0 // NaN
	1 / 0 // Infinity

Infinity有正负之分，Infinity表示正的无穷，-Infinity表示负的无穷。

`Infinity`大于一切数值（除了`NaN`），`-Infinity`小于一切数值（除了`NaN`）。

`Infinity`与`NaN`比较，总是返回`false`。

- 0乘以`Infinity`，返回`NaN`；0除以`Infinity`，返回`0`；`Infinity`除以0，返回`Infinity`。
- `Infinity`加上或乘以`Infinity`，返回的还是`Infinity`。
- `Infinity`减去或除以`Infinity`，得到`NaN`。
- `Infinity`与`null`计算时，`null`会转成0，等同于与`0`的计算。
- `Infinity`与`undefined`计算，返回的都是`NaN`

## 方法 ##
### Number.isFinite() ###
用来检查一个数值是否为有限的（finite），即不是`Infinity`。

注意，如果参数类型不是数值，一律返回`false`。
### Number.isNaN() ###
用来检查一个值是否为`NaN`。

如果参数类型不是`NaN`，一律返回`false`。

### Number.parseInt() ###
`paeseInt`方法用于将字符串转为整数。如果字符串头部有空格，空格会被自动去除。如果`parseInt`的参数不是字符串，则会先转为字符串再转换。字符串转为整数的时候，是一个个字符依次转换，如果遇到不能转为数字的字符，就不再进行下去，返回已经转好的部分。

如果字符串的第一个字符不能转化为数字（后面跟着数字的正负号除外），返回`NaN`。

所以，`parseInt`的返回值只有两种可能，要么是一个十进制整数，要么是`NaN`。

如果字符串以`0x`或`0X`开头，`parseInt`会将其按照十六进制数解析。

对于那些会自动转为科学计数法的数字，`parseInt`会将科学计数法的表示方法视为字符串，因此导致一些奇怪的结果。

	parseInt('1e+21') // 1

`parseInt`方法还可以接受第二个参数（2到36之间），表示被解析的值的进制，返回该值对应的十进制数。默认情况下，`parseInt`的第二个参数为10，即默认是十进制转十进制。

	parseInt('1000', 2) // 8
	parseInt('1000', 6) // 216

如果第二个参数不是数值，会被自动转为一个整数。这个整数只有在2到36之间，才能得到有意义的结果，超出这个范围，则返回NaN。如果第二个参数是0、undefined和null，则直接被忽略。

如果字符串包含对于指定进制无意义的字符，则从最高位开始，只返回可以转换的数值。如果最高位无法转换，则直接返回NaN。

### Number.parseFloat() ###
`parseFloat`方法用于将一个字符串转为浮点数。

### Number.isInteger() ###
用来判断一个数值是否为整数。如果参数不是数值，`Number.isInteger`返回`false`。

如果对数据精度的要求较高，不建议使用`Number.isInteger()`判断一个数值是否为整数。

### Number.isSafeInteger() ###
用来判断一个整数是否落在安全整数这个范围之内。

### Math.pow() ###
`Math.pow()`函数返回基数（base）的指数（exponent）次幂，即base<sup>exponent</sup>。

语法
 
	Math.pow(base, exponent)

参数：

- base：基数
- exponent：指数

### Math.trunc() ###
用于去除一个数的小数部分，返回整数部分。

对于非数值，`Math.trunc`内部使用`Number`方法将其先转为数值。

对于空值和无法截取整数的值，返回`NaN`。

	Math.trunc = Math.trunc || function(x) {
		return x < 0 ? Math.ceil(x) : Math.floor(x);
	};

### Math.sign() ###
用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。

它会返回五种值。

- 参数为正数，返回`+1`；
- 参数为负数，返回`-1`；
- 参数为0，返回`0`；
- 参数为-0，返回`-0`；
- 其他值，返回`NaN`。

### Math.cbrt() ###
用于计算一个数的立方根。

### Math.clz32() ###
JavaScript的整数使用32位二进制形式表示，`Math.clz32`方法返回一个数的32位无符号整数形式有多少个前导0。

### Math.imul() ###
返回两个数以32位带符号整数形式相乘的结果，返回的也是一个32位的带符号整数。

### Math.fround ###
返回一个数的32位单精度浮点数形式。

### Math.hypot() ###
返回所有参数的平方和的平方根

	Math.hypot(3, 4) // 5
	Math.hypor(3, 4, 5) // 7.0710678118654755

### Math.expm1() ###
`Math.expm(x)`返回e<sup>x</sup>-1，即`Math.exp(x) - 1`。

### Math.log1p() ###
`Math.log1p(x)`方法返回`1 + x`的自然对数，即`Math.log(1 + x)`。如果x小于-1，返回NaN。

### Math.log10() ###
`Math.log10(x)`返回以10为底的x的对数。如果x小于0，则返回NaN。

### Math.log2() ###
`Math.log2(x)`返回以2为底的x的对数。如果x小于0，则返回NaN。

### 双曲函数方法 ###
- `Math.sinh(x)`返回`x`的双曲正弦
- `Math.cosh(x)`返回`x`的双曲余弦
- `Math.tanh(x)`返回`x`的双曲正切
- `Math.asinh(x)`返回`x`的反双曲正弦
- `Math.acosh(x)`返回`x`的反双曲余弦
- `Math.atanh(x)`返回`x`的反双曲正切


## 常量 ##
### Number.EPSILON ###
表示1与大于1的最小浮点数之间的差。

	Number.EPSILON === Math.pow(2, -52) // true
`Number.EPSILON`实际上是JavaScript能够表示的最小精度。

误差检查函数

	function withinErrorMargin (left, right) {
		return Math.abs(left - right) < Number.EPSILON * Math.pow(2, 2);
	}
	
	0.1 + 0.2 === 0.3 // false
	withinErrorMargin(0.1 + 0.2, 0.3) // true

### Number.MAX_SAFE_INTEGER和Number.MIN_SAFE_INTEGER ###
JavaScript能够准确表示的整数范围在`-2^53`到`2^53`之间（不含两个端点），超过这个范围，无法精确表示这个值。

ES6 引入了`Number.MAX_SAFE_INTEGER`和`Number.MIN_SAFE_INTEGER`这两个常量，用来表示这个范围的上下限。

## 指数运算符`**` ##
右结合。多个指数运算符连用时，是从最右边开始计算的。

	2 ** 2 // 4
	2 ** 3 // 8

## 一些深度理解数值的知识 ##
JavaScript的Number类型以IEEE 754的标准存储。这意味着每个浮点数占64位。

根据国际标准IEEE 754，JavaScript浮点数的64个二进制位，从最左边开始，是这样组成的。

- 第1位：符号位，`0`表示正数，`1`表示负数
- 第2位到第12位（共11位）：指数部分
- 第13位到第64位（共52）位：小数部分（即有效数字）

符号位决定了一个数的正负，指数部分决定了数值的大小，小数部分决定了数值的精度。

指数部分一共有11个二进制位，因此大小范围就是0到2047。IEEE 754规定，如果指数部分的值在0到2047之间（不含有两个端点），那么有效数字的第一位默认总是1，不保存在64位浮点数之中。也就是说，有效数字这是总是`1.xx...xx`的形式，其中`xx..xx`的部分保存在64位浮点数之中，最长可能为52位。因此，JavaScript提供的有效数字最长为53个二进制位。

### 数值精度 ###
精度最多只能到53个二进制位，这意味着，绝对值小于2的53次方的整数，即-2<sup>53</sup>到2<sup>53</sup>，都可以精确表示。

由于2的53次方是一个16位的十进制数值，所以简单的法则就是，JavaScript对15位的十进制数都可以精确处理。

### 数值范围 ###
根据标准，64位浮点数的指数部分的长度是11个二进制位，意味着指数部分的最大值是2047（2的11次方减1）。也就是说，64位浮点数的指数部分的值最大为2047，分出一半表示负数，则JavaScript能够表示的数值范围为2<sup>1024</sup>到2<sup>-1023</sup>（开区间），超出这个范围的数无法表示。

参考

- [每一个JavaScript开发者应该了解的浮点知识](https://yanhaijing.com/javascript/2014/03/14/what-every-javascript-developer-should-know-about-floating-points/)
- [JavaScript 标准参考教程(基本语法之数值) —— 阮一峰](https://wangdoc.com/javascript/types/number.html)



