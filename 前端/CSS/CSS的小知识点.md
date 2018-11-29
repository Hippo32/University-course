# CSS的小知识点 #

2018/11/6 14:54:37 
	
	position: fixed;
	top: 0; right: 0; bottom: 0; left: 0;
	background-color: rgba(0, 0, 0, .3);
一般做遮盖层的时候用，可以让这个层充满整个屏幕。

2018/11/27 16:13:02 

另一个遮罩层

	width: 100vw;
	height: 100vh;
	position: fixed;
	top: 0;
	left: 0;
	background: #000;
	opacity: 0.5;


----------
2018/11/6 15:15:45 

	display: flex;
	justify-content: center; /* 水平居中 */
	align-items: center; /* 垂直居中 */
flex模型下的垂直水平居中。


----------
2018/11/20 22:55:03 

- 代码易维护和代码量少不可兼得
- currentColor
- 继承（inherit）

----------
2018/11/27 16:08:47 
### 让容器高度充满整个屏幕 ###
	.container {
		min-height: 100vh;
		display: flex;
	}
vh：视窗高度的百分比。

