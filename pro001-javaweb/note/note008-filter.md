[TOC]

# 过滤器笔记

## 1、过滤器三要素

- 拦截：一个请求必须先把它拦住，才能做后续处理
- 过滤：预设的检查条件，满足条件才可以放行
- 放行：对于满足要求的请求，放它过去，让它原本要访问什么资源就继续还是访问那个资源



## 2、过滤器生命周期

| 生命周期环节 | 调用的方法                                                   | 时机                                                         | 次数 |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| 创建对象     | 无参构造器                                                   | <span style="color:blue;font-weight:bold;">默认：Web应用启动时</span> | 一次 |
| 初始化       | init(FilterConfig filterConfig)                              | 创建对象后                                                   | 一次 |
| 处理请求     | doFilter(ServletRequest request, <br />ServletResponse response, <br />FilterChain chain) | 接收到请求后                                                 | 多次 |
| 清理操作     | destroy()                                                    | Web应用卸载之前                                              | 一次 |



## 3、拦截请求时的匹配规则

- 精确匹配
- 模糊匹配★
  - 前杠后星：/user/*
  - 前星后缀：*.html
- 根据Servlet名称匹配



## 4、Filter链

- 概念：拦截同一资源的多个Filter
- 执行顺序：由web.xml中filter-mapping配置的顺序决定
- chain.doFilter(req,resp)：将请求放行到Filter链中的一下一个Filter，如果当前Filter已经是最后一个了，那么直接放行这个请求去访问原本要访问的资源



[返回上一级](index.html)