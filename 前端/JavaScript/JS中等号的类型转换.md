# js中`==`的类型转换 #
2018/10/28 0:01:09 

由`v-if`扩展出来的关于`==`的深究。

1. 首先，要记住**`null == undefined`**始终为`true`，这个暂时没有道理可言，记住就好了。
2. 首先看`==`前后有没有`NaN`，有的话都是返回false。`NaN`不等于任何值，包括其本身。
3. 布尔值会转成数字类型，`true`转成1，`false`转成0。

		true == 1; // true
		false == 0; // false
		true == 2; //false
4. 数字和字符串比较，字符串会转换成数字。

		"123" == 123; // true
		"0" == 0; // true
		"1a" == 1; // false，"1a"转成数字是NaN
5. `undefined`和`null`除了和`undefined`或`null`相等，和其他相比都是`false`

		undefined == undefined; // true
		undefined == false; // true
		undefined == 0; // false
3. 数字或者字符串和对象相比，对象使用`valueOf()`或者`toString()`方法转换。
		
		[] == false; // true
		{} == false; // 报错
		[] == 0; // true
		[1] == 1; // true
5. `==`等于运算符将原始值和其包装对象视为相等，但`===`全等运算符将它们视为不等。

		var a = new Boolean(false);
		a == 0; // true

## ES5规范11.9.3抽象相等比较算法 ##
比较运算`x==y`，其中x和y是值，产生true或者false。这样的比较如下方式进行：

1. 若Type(x)与Type(y)相同，则
	1. 若Type(x)为Undefined，返回true。
	2. 若Type(x)为Null，返回true。
	3. 若Type(x)为Number，则
		1. 若x为NaN，返回false。
		2. 若y为NaN，返回false。
		3. 若x与y为相等数值，返回true。
		4. 若x为+0且y为-0，返回true。
		5. 若x为-0且y为+0，返回true。
		6. 返回false。
	4. 若Type(x)为String，则当x和y为完全相同的字符序列（长度相等且相同字符在相同位置）时返回true。否则，返回false。
	5. 若Type(x)为Boolean，当x和y为同为true或者同为false时返回true。否则，返回false。
	6. 当x和y为引用同一对象时返回true。否则，返回false。
2. 若x为null且y为undefined，返回true。
3. 若x为undefined且y为null，返回true。
4. 若Type(x)为Number且Type(y)为String，返回comparison x == ToNumber(y)的结果。
5. 若Type(x)为String且Type(y)为Number，返回比较ToNumber(x) == y的结果。
6. 若Type(x)为Boolean，返回比较ToNumber(x) == y的结果。
7. 若Type(y)为Boolean，返回比较x == ToNumber(y)的结果。
8. 若Type(x)为String或Number，且Type(y)为Object，返回比较x == ToPrimitive(y)的结果。
9. 若Type(x)为Object且Type(y)为String或Number，返回比较ToPrimitive(x) == y的结果。
10. 返回false。

注：相等运算符不总是传递的。例如，两个不同的String对象，都表示相同的字符串值；==运算符认为每个String对象都与字符串值相等，但是两个字符串值对象互不相等。例如：
	
- new String("a") == "a" 和 "a" == new String("a")皆为true。
- new String("a") == new String("a")为false。


参考：

- [ES5规范11.9.3抽象相等比较算法](http://lzw.me/pages/ecmascript/#203)
- 《你不知道的JavaScript（中卷）》4.5.2抽象相等
- [javascript双等号引起的类型转换，js隐性类型转换步骤](https://www.haorooms.com/post/js_yinxingleixing)