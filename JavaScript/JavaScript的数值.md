# JavaScript的数值 #
## 二进制和八进制表示法 ##
`0b` —— 二进制

`0o` —— 八进制

如果要将`0b`和`0o`前缀的字符串数值转为十进制，要使用`Number`方法。
	
	Number('0b111') // 7

## 方法 ##
### Number.isFinite() ###
用来检查一个数值是否为有限的（finite），即不是`Infinity`。

注意，如果参数类型不是数值，一律返回`false`。
### Number.isNaN() ###
用来检查一个值是否为`NaN`。

如果参数类型不是`NaN`，一律返回`false`。

### Number.parseInt() ###

### Number.parseFloat() ###

### Number.isInteger() ###
用来判断一个数值是否为整数。如果参数不是数值，`Number.isInteger`返回`false`。

如果对数据精度的要求较高，不建议使用`Number.isInteger()`判断一个数值是否为整数。

### Number.isSafeInteger() ###
用来判断一个整数是否落在安全整数这个范围之内。

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



