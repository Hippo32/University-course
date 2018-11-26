# CSS字体 #
## font-size ##
常见设置字体大小有px，rem，em，百分比。

	font-size: 100%;
设置字体属性为默认大小，是相对于浏览器默认字体大小或继承body设定的字体大小来说的。

chrome最小字体大小12px  
在所有现代浏览器中，其默认的字体大小就是16px。

em指的是相对于元素父元素的font-size


当body字体为16px时:

|Pixels|EMs|Percent|Points|
|-|-|-|-|
|16px|1em|100%|12pt|

如果想简单一点，可以将body的font-size设为62.5%，这样将你原来的px数值除以10，就是em的值。