[TOC]

# 第三节 进入后台开发

## 1、概念辨析

![images](images/img004.png)



## 2、访问后台首页

### ①思路

首页→后台系统超链接→AdminServlet.toPortalPage()→manager.html



### ②实现：创建AdminServlet

web.xml

```xml
<servlet>
    <servlet-name>AdminServlet</servlet-name>
    <servlet-class>com.atguigu.bookstore.servlet.model.AdminServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>AdminServlet</servlet-name>
    <url-pattern>/AdminServlet</url-pattern>
</servlet-mapping>
```

Java代码：

```java
public class AdminServlet extends ModelBaseServlet {
    protected void toPortalPage(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        String viewName = "manager/manager";
        
        processTemplate(viewName, request, response);
        
    }
}
```



### ③实现：调整manager.html

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <base th:href="@{/}" href="/bookstore/"/>
```

然后去除页面上的所有“../”。



### ④实现：抽取页面公共部分

#### [1]公共部分内容

三个超链接：

```html
          <a href="./book_manager.html" class="order">图书管理</a>
          <a href="./order_manager.html" class="destory">订单管理</a>
          <a href="../../index.html" class="gohome">返回商城</a>
```



#### [2]抽取它们的理由

为了实现链接地址修改时：一处修改，处处生效



#### [3]创建包含代码片段的页面

![images](images/img005.png)

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

    <!-- 使用th:fragment属性给代码片段命名 -->
    <div th:fragment="navigator">
        <a href="book_manager.html" class="order">图书管理</a>
        <a href="order_manager.html" class="destory">订单管理</a>
        <a href="index.html" class="gohome">返回商城</a>
    </div>

</body>
</html>
```



#### [4]在有需要的页面引入片段

```html
<div th:include="segment/admin-navigator :: navigator"></div>
```



[上一节](verse02.html) [回目录](index.html) [下一节](verse04.html)