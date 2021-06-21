[TOC]



# 第九节 Vue.js基本语法：侦听属性

## 1、提出需求

```html
<div id="app">
	<p>尊姓：{{firstName}}</p>
	<p>大名：{{lastName}}</p>
	尊姓：<input type="text" v-model="firstName" /><br/>
	大名：<input type="text" v-model="lastName" /><br/>
	<p>全名：{{fullName}}</p>
</div>
```

在上面代码的基础上，我们希望firstName或lastName属性发生变化时，修改fullName属性。此时需要对firstName或lastName属性进行<span style="color:blue;font-weight:bold;">『侦听』</span>。

具体来说，所谓<span style="color:blue;font-weight:bold;">『侦听』</span>就是对message属性进行监控，当firstName或lastName属性的值发生变化时，调用我们准备好的函数。



## 2、Vue代码

在watch属性中声明对firstName和lastName属性进行<span style="color:blue;font-weight:bold;">『侦听』</span>的函数：

```javascript
var app = new Vue({
	"el":"#app",
	"data":{
		"firstName":"jim",
		"lastName":"green",
		"fullName":"jim green"
	},
	"watch":{
		"firstName":function(inputValue){
			this.fullName = inputValue + " " + this.lastName;
		},
		"lastName":function(inputValue){
			this.fullName = this.firstName + " " + inputValue;
		}
	}
});
```



[上一节](verse08.html) [回目录](index.html) [下一节](verse10.html)