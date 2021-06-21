[TOC]

# 第八节 Vue.js基本语法：事件驱动

## 1、demo：字符串顺序反转

### ①HTML代码

```html
<div id="app">
	<p>{{message}}</p>
	
	<!-- v-on:事件类型="事件响应函数的函数名" -->
	<button v-on:click="reverseMessage">Click me,reverse message</button>
</div>
```



### ②Vue代码

```javascript
var app = new Vue({
	"el":"#app",
	"data":{
		"message":"Hello Vue!"				
	},
	"methods":{
		"reverseMessage":function(){
			this.message = this.message.split("").reverse().join("");
		}
	}
});
```



## 2、demo：获取鼠标移动时的坐标信息

### ①HTML代码

```html
<div id="app">
	<div id="area" v-on:mousemove="recordPosition"></div>
	<p id="showPosition">{{position}}</p>
</div>
```



### ②Vue代码

```javascript
var app = new Vue({
	"el":"#app",
	"data":{
		"position":"暂时没有获取到鼠标的位置信息"
	},
	"methods":{
		"recordPosition":function(event){
			this.position = event.clientX + " " + event.clientY;
		}
	}
});
```



[上一节](verse07.html) [回目录](index.html) [下一节](verse09.html)