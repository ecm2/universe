[TOC]



# 第四节 Vue.js基本语法：绑定元素属性

## 1、基本语法

v-bind:HTML标签的原始属性名



## 2、demo

### ①HTML代码

```html
<div id="app">
	<!-- v-bind:value表示将value属性交给Vue来进行管理，也就是绑定到Vue对象 -->
	<!-- vueValue是一个用来渲染属性值的表达式，相当于标签体中加{{}}的表达式 -->
	<input type="text" v-bind:value="vueValue" />
	
	<!-- 同样的表达式，在标签体内通过{{}}告诉Vue这里需要渲染； -->
	<!-- 在HTML标签的属性中，通过v-bind:属性名="表达式"的方式告诉Vue这里要渲染 -->
	<p>{{vueValue}}</p>
</div>

```



### ②Vue代码

```javascript
// 创建Vue对象，挂载#app这个div标签
var app = new Vue({
	"el":"#app",
	"data":{
		"vueValue":"太阳当空照"
	}
});
```



## 3、小结

本质上{{表达式}}，v-bind:属性名="表达式"它们都是<span style="color:blue;font-weight:bold;">用Vue对象来渲染页面</span>。只不过：

- 文本标签体：使用{{表达式}}形式
- 属性：使用v-bind:属性名="表达式"形式



[上一节](verse03.html) [回目录](index.html) [下一节](verse05.html)