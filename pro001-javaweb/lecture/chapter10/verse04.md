[TOC]

# 第四节 过滤器匹配规则

本节要探讨的是在filter-mapping中如何将Filter同它要拦截的资源关联起来。



## 1、精确匹配

指定被拦截资源的完整路径：

```xml
<!-- 配置Filter要拦截的目标资源 -->
<filter-mapping>
    <!-- 指定这个mapping对应的Filter名称 -->
    <filter-name>Target01Filter</filter-name>

    <!-- 通过请求地址模式来设置要拦截的资源 -->
    <url-pattern>/Target01Servlet</url-pattern>
</filter-mapping>
```



## 2、模糊匹配

相比较精确匹配，使用模糊匹配可以让我们创建一个Filter就能够覆盖很多目标资源，不必专门为每一个目标资源都创建Filter，提高开发效率。

### ①前杠后星

在我们配置了url-pattern为/user/*之后，请求地址只要是/user开头的那么就会被匹配。

```xml
<filter-mapping>
    <filter-name>Target02Filter</filter-name>

    <!-- 模糊匹配：前杠后星 -->
    <!--
        /user/Target02Servlet
        /user/Target03Servlet
        /user/Target04Servlet
    -->
    <url-pattern>/user/*</url-pattern>
</filter-mapping>
```

<span style="color:blue;font-weight:bold;">极端情况：/*匹配所有请求</span>

### ②前星后缀

下面我们使用png图片来测试后缀拦截的效果，并不是只能拦截png扩展名。

#### [1]创建一组img标签

```html
    <img th:src="@{/images/img017.png}"/><br/>
    <img th:src="@{/images/img018.png}"/><br/>
    <img th:src="@{/images/img019.png}"/><br/>
    <img th:src="@{/images/img020.png}"/><br/>
    <img th:src="@{/images/img024.png}"/><br/>
    <img th:src="@{/images/img025.png}"/><br/>
```

#### [2]创建Filter

```xml
<filter>
    <filter-name>Target04Filter</filter-name>
    <filter-class>com.atguigu.filter.filter.Target04Filter</filter-class>
</filter>
<filter-mapping>
    <filter-name>Target04Filter</filter-name>
    <url-pattern>*.png</url-pattern>
</filter-mapping>
```

### ③前杠后缀，星号在中间

配置方式如下：

```xml
<url-pattern>/*.png</url-pattern>
```

按照这个配置启动Web应用时会抛出异常：

> java.lang.IllegalArgumentException: Invalid <url-pattern> /*.png in filter mapping

<span style="color:blue;font-weight:bold;">结论：这么配是<span style="color:red;font-weight:bold;">不允许</span>的！</span>

## 3、匹配Servlet名称[了解]

```xml
<filter-mapping>
    <filter-name>Target05Filter</filter-name>

    <!-- 根据Servlet名称匹配 -->
    <servlet-name>Target01Servlet</servlet-name>
</filter-mapping>
```



[上一节](verse03.html) [回目录](index.html) [下一节](verse05.html)