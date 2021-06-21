[TOC]

# 第一节 事件驱动补充

## 1、取消控件的默认行为

### ①控件默认行为

- 点超链接会跳转页面
- 点表单提交按钮会提交表单

本来控件的默认行为是天经地义就该如此的，但是如果我们希望点击之后根据我们判断的结果再看是否要跳转，此时默认行为无脑跳转的做法就不符合我们的预期了。



### ②取消方式

调用<span style="color:blue;font-weight:bold;">事件对象</span>的<span style="color:blue;font-weight:bold;">preventDefault()</span>方法。



#### [1]超链接举例

HTML代码：

```html
<a id="anchor" href="http://www.baidu.com">超链接</a>
```

JavaScript代码：

```javascript
document.getElementById("anchor").onclick = function() {
	console.log("我点击了一个超链接");
	event.preventDefault();
}
```



#### [2]表单提交按钮举例

HTML代码：

```html
<form action="http://www.baidu.com" method="post">
	<button id="submitBtn" type="submit">提交表单</button>
</form>
```

JavaScript代码：

```javascript
document.getElementById("submitBtn").onclick = function() {
	console.log("我点击了一个表单提交按钮");
	event.preventDefault();
}
```



## 2、阻止事件冒泡

![images](images/img001.png)

图中的两个div，他们的HTML标签是：

```html
<div id="outterDiv">
	<div id="innerDiv"></div>
</div>
```

点击里面的div同时也等于点击了外层的div，此时如果两个div上都绑定了单击响应函数那么就都会被触发：

```javascript
document.getElementById("outterDiv").onclick = function() {
	console.log("外层div的事件触发了");
}

document.getElementById("innerDiv").onclick = function() {
	console.log("内层div的事件触发了");
}

```

所以事件冒泡就是一个事件会不断向父元素传递，直到window对象。

如果这不是我们想要的效果那么可以使用<span style="color:blue;font-weight:bold;">事件对象</span>的<span style="color:blue;font-weight:bold;">stopPropagation()</span>函数阻止。

```javascript
document.getElementById("innerDiv").onclick = function() {
	console.log("内层div的事件触发了");
	
	event.stopPropagation();
}
```



## 3、Vue事件修饰符

对于事件修饰符，Vue官网的描述是：

> 在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：<span style="color:blue;font-weight:bold;">方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节</span>。



### ①取消控件的默认行为

控件的默认行为指的是：

- 点击超链接跳转页面
- 点击表单提交按钮提交表单

实现这个需求使用的Vue事件修饰符是：<span style="color:blue;font-weight:bold;">.prevent</span>

```html
<a href="http://www.baidu.com" @click.prevent="clickAnchor">超链接</a>

<form action="http://www.baidu.com" method="post">
	<button type="submit" @click.prevent="clickSubmitBtn">提交表单</button>
</form>
```



### ②取消事件冒泡

实现这个需求使用的Vue事件修饰符是：<span style="color:blue;font-weight:bold;">.stop</span>

```html
<div id="outterDiv" @click="clickOutterDiv">
	<div id="innerDiv" @click.stop="clickInnerDiv"></div>
</div>
```



[回目录](index.html) [下一节](verse02.html)