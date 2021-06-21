# 第五节 使用IDEA创建Servlet

![images](images/img022.png)

![images](images/img023.png)

```xml
<!-- IDEA会自动生成servlet标签 -->
<servlet>
    <servlet-name>QuickServlet</servlet-name>
    <servlet-class>com.atguigu.servlet.QuickServlet</servlet-class>
</servlet>

<!-- 我们自己补充servlet-mapping标签 -->
<servlet-mapping>
    <servlet-name>QuickServlet</servlet-name>
    <url-pattern>/QuickServlet</url-pattern>
</servlet-mapping>
```

IDEA生成的Servlet自动继承了HttpServlet

```java
public class QuickServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

下面是测试代码：

```java
public class QuickServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.getWriter().write("doPost method processed");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.getWriter().write("doGet method processed");
    }
}
```

```html
<form action="/app/QuickServlet" method="get">
    <button type="submit">发送GET请求</button>
</form>

<br/>

<form action="/app/QuickServlet" method="post">
    <button type="submit">发送POST请求</button>
</form>
```

[上一节](verse04.html) [回目录](index.html) [下一节](verse06.html)