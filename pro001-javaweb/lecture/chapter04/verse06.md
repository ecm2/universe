[TOC]

# 第六节 Vue.js基本语法：条件渲染

根据Vue对象中，数据属性的值来判断是否对HTML页面内容进行渲染。



## 1、v-if

### ①HTML代码

```html
<div id="app">
	<h3>if</h3>
	<img v-if="flag" src="/pro03-vue/images/one.jpg" />
	<img v-if="!flag" src="/pro03-vue/images/two.jpg" />
</div>
```



### ②Vue代码

```javascript
		var app = new Vue({
			"el":"#app",
			"data":{
				"flag":true
			}
		});
```



## 2、v-if和v-else

### ①HTML代码

```html
<div id="app02">
	<h3>if/else</h3>
	<img v-if="flag" src="/pro03-vue/images/one.jpg" />
	<img v-else="flag" src="/pro03-vue/images/two.jpg" />
</div>
```



### ②Vue代码

```javascript
		var app02 = new Vue({
			"el":"#app02",
			"data":{
				"flag":true
			}
		});
```



## 3、v-show

### ①HTML代码

```html
<div id="app03">
	<h3>v-show</h3>
	<img v-show="flag" src="/pro03-vue/images/mi.jpg" />
</div>
```



### ②Vue代码

```javascript
		var app03 = new Vue({
			"el":"#app03",
			"data":{
				"flag":true
			}
		});
```



[上一节](verse05.html) [回目录](index.html) [下一节](verse07.html)