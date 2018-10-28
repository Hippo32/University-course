# CSS关于换行or溢出 #
## word-wrap ##
word-wrap属性允许长单词或URL地址换行到下一行。

|-|-|
|默认值|normal|
|继承性|yes|
|版本|CSS3|
|JavaScript语法|object.style.wordWrap="break-word"|

### 值 ###
- `normal`：只在允许的断字点换行（浏览器保持默认处理）。
- `break-word`：在长单词或URL地址内部进行换行。强制让文字换行。一般情况下当父级宽度不够的时候，不管英文单词自动换行是当一整个单词不够放时，整个单词一起换行到下一行。兼容所有浏览器。

## word-break ##
`word-break`指定了怎样在单词内断行

|-|-|
|初始值|normal|
|是否是继承属性|yes|


### 值 ###
- `normal`：使用默认的断行规则。
- `break-all`：对于non-CJK（CJK指中文/日文/韩文）文本，可在任意字符间断行。只不兼容opera，其他浏览器都兼容。
- `keep-all`：CJK文本不断行。Non-CJK文本表现同normal。

## white-space ##
`white-space`CSS属性是用来设置如何处理元素中的空白

|-|-|
|初始值|normal|
|是否是继承属性|yes|

### 值 ###
- `noraml`：连续的空白符会被合并，换行符会被当作空白符来处理。填充line盒子时，必要的话会换行。
- `nowrap`：和normal一样，连续的空白符会被合并。但文本内的换行无效。
- `pre`：连续的空白符会被保留。在遇到换行符或者`<br>`元素时才会换行。
- `pre-wrap`：连续的空白符会被保留。在遇到换行符