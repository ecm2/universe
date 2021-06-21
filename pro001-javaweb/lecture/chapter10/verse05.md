[TOC]

# 第五节 过滤器链

## 1、概念

- 多个Filter的<span style="color:blue;font-weight:bold;">拦截范围</span>如果存在<span style="color:blue;font-weight:bold;">重合部分</span>，那么这些Filter会形成<span style="color:blue;font-weight:bold;">Filter链</span>。
- 浏览器请求重合部分对应的目标资源时，会<span style="color:blue;font-weight:bold;">依次经过</span>Filter链中的每一个Filter。
- Filter链中每一个Filter执行的<span style="color:blue;font-weight:bold;">顺序是由web.xml中filter-mapping配置的顺序决定</span>的。

![images](images/img004.png)

## 2、测试

### ①准备工作

创建超链接访问一个普通的Servlet即可。

### ②创建多个Filter拦截Servlet

```xml
<filter-mapping>
    <filter-name>TargetChain03Filter</filter-name>
    <url-pattern>/Target05Servlet</url-pattern>
</filter-mapping>
<filter-mapping>
    <filter-name>TargetChain02Filter</filter-name>
    <url-pattern>/Target05Servlet</url-pattern>
</filter-mapping>
<filter-mapping>
    <filter-name>TargetChain01Filter</filter-name>
    <url-pattern>/Target05Servlet</url-pattern>
</filter-mapping>
```

控制台打印效果：

> 过滤器执行：Target03Filter[模糊匹配 前杠后星 /*]
> 测试Filter链：TargetChain03Filter
> 测试Filter链：TargetChain02Filter
> 测试Filter链：TargetChain01Filter



[上一节](verse04.html) [回目录](index.html)