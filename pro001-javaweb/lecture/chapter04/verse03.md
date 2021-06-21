[TOC]



# 第三节 Vue.js基本语法：声明式渲染



## 1、概念

### ①声明式

<span style="color:blue;font-weight:bold;">『声明式』</span>是相对于<span style="color:blue;font-weight:bold;">『编程式』</span>而言的。

- 声明式：告诉框架做什么，具体操作由框架完成
- 编程式：自己编写代码完成具体操作



### ②渲染

![images](images/img003.png)

上图含义解释：

- 蓝色方框：HTML标签
- 红色圆形：动态、尚未确定的数据
- 蓝色圆形：经过程序运算以后，计算得到的具体的，可以直接在页面上显示的数据、
- 渲染：程序计算动态数据得到具体数据的过程



## 2、demo

### ①HTML代码

```html
		<!-- 使用{{}}格式，指定要被渲染的数据 -->
		<div id="app">{{message}}</div>
```



### ②vue代码

```javascript
// 1.创建一个JSON对象，作为new Vue时要使用的参数
var argumentJson = {
	
	// el用于指定Vue对象要关联的HTML元素。el就是element的缩写
	// 通过id属性值指定HTML元素时，语法格式是：#id
	"el":"#app",
	
	// data属性设置了Vue对象中保存的数据
	"data":{
		"message":"Hello Vue!"
	}
};

// 2.创建Vue对象，传入上面准备好的参数
var app = new Vue(argumentJson);
```

![images](images/img004.png)



## 3、查看声明式渲染的响应式效果

![images](images/img005.png)

通过验证Vue对象的『响应式』效果，我们看到Vue对象和页面上的HTML标签确实是始终保持着关联的关系，同时看到Vue在背后确实是做了大量的工作。



[上一节](verse02.html) [回目录](index.html) [下一节](verse04.html)