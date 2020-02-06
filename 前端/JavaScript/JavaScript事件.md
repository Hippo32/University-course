# JavaScript事件 #
## 事件流 ##
IE和Netscape开发团队的不同理解

IE —— 事件冒泡 —— 事件开始时最具体的元素（文档中嵌套层次最深的那个节点）接收，然后逐级向上传播到较为不具体的节点（文档）

Netscape —— 事件捕获 —— 不太具体的节点应该更早接收到事件，而最具体的节点应该最后接收到节点

## 事件处理程序 ##
### 在HTML中指定事件处理程序 ###
缺点

1. 时差问题。用户可能会在HTML元素一出现在页面上就触发相应的事件，但当时的事件处理程序有可能尚不具备执行条件。
2. 扩展事件处理程序的作用域链在不同浏览器中会导致不同结果。
3. HTML与JavaScript代码紧密耦合

### 在JS中指定事件处理程序 ###
DOM0级： `btn.onclick = null;`  
DOM2级：`btn.addEventListener("click", function(){
alert(this.id);
}, false);`  
IE事件：`btn.attachEvent("onclick", function(){
alert("Clicked");
});`

>删除事件处理程序  
>`btn.onclick = null;`

`addEventListener()`和`removeEventListener()`所有 DOM 节点中都包含这两个方法，并且它们都接受 3 个参数：**要处理的事件名**、**作为事件处理程序的函数**和**一个布尔值**。最后这个布尔值参数如果是 true ，表示在捕获阶段调用事件处理程序；如果是 false ，表示在冒泡阶段调用事件处理程序。

大多数情况下，都是将事件处理程序添加到事件流的冒泡阶段，这样可以最大限度地兼容各种浏览器。

### IE事件处理程序 ###
`attachEvent()`和`detachEvent()`。这两个方法接受相同的参数：事件处理程序名称与事件处理程序函数。

在IE中使用`attachEvent()`的作用域是window。

可以为一个元素添加一个多个事件处理程序，不过执行顺序是**后添加的先执行**

## 事件对象 ##
事件对象即event对象
### DOM中的事件对象 ###
属性/方法

![](https://i.imgur.com/m6B8EOT.png)

![](https://i.imgur.com/gNH4Pa1.png)

阻止特定事件的默认行为，可以使用`preventDefault()`方法。  
只有`cancelable`属性设置为true的事件，才可以使用`preventDefault()`来取消其默认行为。

`stopPropagation()`方法用于立即停止事件在DOM层级中的传播，即取消进一步的事件捕获或冒泡。

`eventPhase`实行，可以用来确定事件当前正处于事件流的哪个阶段。如果是在捕获阶
段调用的事件处理程序，那么 eventPhase 等于 1 ；如果事件处理程序处于目标对象上，则 eventPhase 等于 2 ；如果是在冒泡阶段调用的事件处理程序， eventPhase 等于 3 。这里要注意的是，尽管“处于目标”发生在冒泡阶段，但 eventPhase 仍然一直等于 2 。



### IE中的事件对象 ###
所有事件对象都会包含下表所列的属性和方法

![](https://i.imgur.com/iUSzT50.png)
 
>DOM中的`event.target`等于IE中的`window.event.srcElement`  
> 
>`returnValue`属性相当于DOM中的`preventDefault()`方法，它们的作用都是取消给定事件的默认行为。只要将`returnValue`设置为`false`，就可以阻止默认行为。
>  
>`cancelBubble`属性与DOM中的`stopPropagation()`方法作用相同，都是用来**停止事件冒泡**的。由于IE不支持事件捕获，因为只能取消事件冒泡；但`stopPropagation()`可以同时**取消事件捕获和冒泡**。

### 跨浏览器的事件对象 ###

	event = event | window.event;
	target = event.target | event.srcElement;
	prventDefault: function(event) {
		if(event.preventDefault) {
			event.preventDefault();
		} else {
			event.returnValue = false;
		}
	}

	stopPropagation: function(event) {
		if(event.stopPropagation) {
			event.stopPropagation();
		} else {
			event.cancelBubble = true;
		}
	}

	addHandler: function(element, type, handler) {
		if(element.addEventListener) {
			element.addEventListener(type, handler, false);
		} else if (element.attachEvent) {
			element.attachEvent("on" + type, handler);
		} else {
			element["on" + type] = handler;
		}
	}

## 事件类型 ##
- UI事件，当用户与页面上的元素交互时触发
	- load事件
	- unload事件
	- resize事件
	- scroll事件
- 焦点事件，当元素获得或失去焦点时触发
	- blur：在元素失去焦点时触发。
	- focus：在元素获得焦点时触发。不会冒泡。
	- focusin：在元素获得焦点时触发。冒泡。
	- focusout：在元素失去焦点时触发。
- 鼠标事件
	- click：在用户单击主鼠标按钮（一般是左边的按钮）或者按下回车键时触发。DOM的button属性可能有如下3个值：0表示主鼠标按钮，1表示中间的鼠标按钮（鼠标滚轮按钮），2表示次鼠标按钮。
	- dblclick：在用户双击主鼠标按钮（一般是左边的按钮）时触发。
	- mousedown：在用户按下了任意鼠标按钮时触发。
	- mouseenter：在鼠标光标从元素外部首次移动到元素范围之内时触发。不会冒泡。
	- mouseleave：在位于元素上方的鼠标光标移动到元素范围之外时触发。不会冒泡。
	- mousemove：当鼠标指针在元素内部移动时重复地触发。
	- mouseout：在鼠标指针位于一个元素上方，然后用于将其移入另一个元素时触发。
	- mouseover：在鼠标指针位于一个元素外部，然后用户将其首次移入另一个元素之内时触发。
	- mouseup：在用户释放鼠标按钮时触发。

	客户区坐标位置：`clientX`和`clientY`，相对于浏览器窗口

	页面坐标位置：`pageX`和`pageY`，相对于页面

	在页面没有滚动的情况下，pageX和pageY的值与clientX和clientY的值相等。

	屏幕坐标位置：`screenX`和`screenY`，相对于整个电脑屏幕
- 滚轮事件
	- mousewheel：当用户通过鼠标滚轮与页面交互、在垂直方向上滚动页面时（无论向上还是向下），就会触发mousewheel事件。

	event属性`wheelDelta`属性。当用户向前滚动鼠标滚轮时，wheelDelta是120的倍数；当用户向后滚动鼠标滚轮时，wheelDelta是-120的倍数。

	Firefox支持一个名为`DOMMouseScroll`的类似事件。而有关鼠标滚轮的信息则保存在`detail`属性中，当向前滚动鼠标滚轮时，这个属性的值是-3的倍数，当向后滚动鼠标滚轮时，这个属性的值是3的倍数。
- 文本事件，当在文档中输入文本时触发
- 键盘事件
	- keydown ：当用户按下键盘上的任意键时触发，而且如果按住不放的话，会重复触发此事件。
	- keypress ：当用户按下键盘上的字符键时触发，而且如果按住不放的话，会重复触发此事件。按下 Esc 键也会触发这个事件。
	- keyup ：当用户释放键盘上的键时触发。

	 keydown 和 keypress 都是在文本框发生变化之前被触发的；而 keyup事件则是在文本框已经发生变化之后被触发的。

	- 键码：keyCode
	- 字符编码：charCode
	- 相应键的名：key
	- char：在按下字符键时的行为与key相同，但在按下非字符键时值为null
	- location：一个数值，表示按下了什么位置上的键：0表示默认键盘，1表示左侧位置，2表示右侧位置，3表示数字小键盘，4表示移动设备键盘（也就是虚拟键盘），5表示手柄。（不推荐使用）
- 合成事件，当为IME（Input Method Editor，输入法编辑器）输入字符时触发
- 变动事件，当底层DOM结构发生变化时触发