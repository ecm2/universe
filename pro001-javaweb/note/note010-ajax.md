[TOC]

# Ajax笔记

## 1、同步和异步

- 同步：串行，各个操作按顺序执行，前面的操作还没有完成的时候，后面的操作就不能进行。有可能会发生阻塞（阻塞对应的英文单词是block）。
- 异步：并行，各个操作在各自的进程或线程中执行，彼此不需要等待。



## 2、Ajax概念本身

- 异步：在浏览器内部有多个线程，有的线程负责展示当前页面，有的线程负责发送Ajax请求。
- 数据传输格式：
  - XML：很久以前使用，现在看来效率太低。
  - JSON：非常优秀的数据传输格式，简洁高效。



## 3、axios的使用

### ①导入axios的环境

```html
<script type="text/javascript" src="/demo/scripts/axios.js"></script>
```



### ②config对象

```javascript
axios(config对象)
```

config对象可以使用的属性名：

| 属性名 | 作用                                                         |
| ------ | ------------------------------------------------------------ |
| method | 给axios的异步请求设置请求方式                                |
| url    | 请求的目标地址                                               |
| params | 请求参数，axios会把params中指定的请求参数附着到URL地址后面   |
| data   | 请求体JSON格式，axios会把data指定的数据整体作为请求体，<br />此时请求体是Request payload格式 |



### ③then()方法

在服务器端成功返回响应数据时调用，并在调用then()中回调函数时传入response对象。response对象的属性名如下：

| 属性名     | 作用                 |
| ---------- | -------------------- |
| data       | 响应体数据           |
| status     | 响应状态码           |
| statusText | 响应状态码的说明文本 |



### ④catch()方法

在服务器端处理请求失败时调用，并在调用catch()中回调函数时传入error对象，error对象包含response属性，结构和上面提到的response对象一样。例如：

```javascript
// 获取响应状态码
error.response.data.status
```



## 4、axios和Vue结合使用

- 结合Vue对象的生命周期：比如在<span style="color:blue;font-weight:bold;">mounted环节</span>，向服务器发送请求获取数据，然后再将数据交给Vue对象，让Vue对象来渲染页面。
- 结合Vue对象对事件的监听：比如在Vue对象的<span style="color:blue;font-weight:bold;">事件响应函数</span>中发送请求获取数据。



[返回上一级](index.html)