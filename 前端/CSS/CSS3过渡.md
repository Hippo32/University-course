# CSS3过渡 #
CSS3过渡是元素从一种样式逐渐改变为另一种的效果。

要实现这一点，必须规定两项内容：

- 规定您希望把效果添加到哪个CSS属性上
- 规定效果的时长

过渡属性：

|属性|描述|
|-|-|
|transition|简写属性，用于在一个属性中设置四个过渡属性。|
|transition-pproperty|规定应用过渡的CSS属性的名称|
|transition-duration|定义过渡效果花费的时间。默认是0。|
|transition-timing-function|规定过渡效果的时间曲线。默认是"ease"。|
|transition-delay|规定过渡效果何时开始。默认是0。|

> 请始终设置transition-duration属性，否则时长为0，就不会产生过渡效果。

## transition-timing-function ##
|值|描述|
|-|-|
|`linear`|规定以相同速度开始至结束的过渡效果（等于cubic-bezier(0,0,1,1))。|
|`ease`|规定慢速开始，然后变快，然后慢速结束的过渡效果(cubic-bezier(0.25,0.1,0.25,0.1))。|
|`ease-in`|规定以慢速开始的过渡效果（等于cubic-bezier(0.42,0,1,1))。|
|`ease-out`|规定以慢速结束的过渡效果（等于cubic-bezier(0,0,0.58,1))。|
|`ease-in-out`|规定以慢速开始和结束的过渡效果（等于cubic-bezier(0.42,0,0.58,1)）。|
|`cubic-bezier(n,n,n,n)`|在cubic-bezier函数中定义自己的值。可能的值是0至1之间的数值。|

# CSS3变换 #
## translate() 2D平移 ##
## rotate() 2D旋转 ##
## scale() 元素的缩放 ##
## skew() 元素的倾斜 ##

# CSS3动画 #
## @keyframes规则 ##
`@keyframes`规则用于创建动画。在`@keyframes`中规定某项CSS样式，就能创建由当前样式逐渐改为新样式的动画效果。