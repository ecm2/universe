[TOC]

# 第二节 Cookie的工作机制

## 1、HTTP协议和会话控制

HTTP协议本身是无状态的。单靠HTTP协议本身无法判断一个请求来自于哪一个浏览器，所以也就没法识别用户的身份状态。

![images](images/img004.png)

## 2、Cookie介绍

### ①本质

- 在浏览器端临时存储数据
- 键值对
- 键和值都是字符串类型
- 数据量很小

### ②Cookie在浏览器和服务器之间的传递

#### [1]没有Cookie的状态

在服务器端没有创建Cookie并返回的情况下，浏览器端不会保存Cookie信息。双方在请求和响应的过程中也不会携带Cookie的数据。

#### [2]创建Cookie对象并返回

```java
// 1.创建Cookie对象
Cookie cookie = new Cookie("cookie-message", "hello-cookie");

// 2.将Cookie对象添加到响应中
response.addCookie(cookie);

// 3.返回响应
processTemplate("page-target", request, response);
```

#### [3]服务器端返回Cookie的响应消息头

![images](images/img005.png)

#### [4]浏览器拿到Cookie之后

浏览器拿到Cookie之后，以后的每一个请求都会携带Cookie信息。

![images](images/img006.png)

#### [5]服务器端读取Cookie的信息

```java
// 1.通过request对象获取Cookie的数组
Cookie[] cookies = request.getCookies();

// 2.遍历数组
for (Cookie cookie : cookies) {
    System.out.println("cookie.getName() = " + cookie.getName());
    System.out.println("cookie.getValue() = " + cookie.getValue());
    System.out.println();
}
```

### ③Cookie时效性

#### [1]理论

- 会话级Cookie
  - 服务器端并没有明确指定Cookie的存在时间
  - 在浏览器端，Cookie数据存在于内存中
  - 只要浏览器还开着，Cookie数据就一直都在
  - 浏览器关闭，内存中的Cookie数据就会被释放
- 持久化Cookie
  - 服务器端明确设置了Cookie的存在时间
  - 在浏览器端，Cookie数据会被保存到硬盘上
  - Cookie在硬盘上存在的时间根据服务器端限定的时间来管控，不受浏览器关闭的影响
  - 持久化Cookie到达了预设的时间会被释放

服务器端返回Cookie时附带过期时间的响应消息头如下：

![images](images/img007.png)

服务器通知浏览器删除Cookie时的响应消息头如下：

![images](images/img009.png)

#### [2]代码

```java
// ※给Cookie设置过期时间
// 正数：Cookie的过期时间，以秒为单位
// 负数：表示这个Cookie是会话级的Cookie，浏览器关闭时释放
// 0：通知浏览器立即删除这个Cookie
cookie.setMaxAge(20);
```

#### [3]会话和持久化Cookie对比

![images](images/img008.png)

### ④Cookie的domain和path

上网时间长了，本地会保存很多Cookie。对浏览器来说，访问互联网资源时不能每次都把所有Cookie带上。浏览器会使用Cookie的domain和path属性值来和当前访问的地址进行比较，从而决定是否携带这个Cookie。

![images](images/img010.png)



[上一节](verse01.html) [回目录](index.html) [下一节](verse03.html)