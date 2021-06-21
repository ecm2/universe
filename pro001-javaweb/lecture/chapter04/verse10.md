[TOC]



# 第十节 Vue.js基本语法：简化写法



## 1、v-bind的简化写法

正常写法：

```html
<input type="text" v-bind:value="message" />
```

简化以后：

```html
<input type="text" :value="message" />
```



## 2、v-on的简化写法

正常写法：

```html
<button v-on:click="sayHello">SayHello</button>
```

简化以后：

```html
<button @click="sayHello">SayHello</button>
```



[上一节](verse09.html) [回目录](index.html) [下一节](verse11.html)