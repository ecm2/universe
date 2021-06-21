[TOC]

# 第三节 过滤器生命周期

## 1、回顾Servlet生命周期

[Servlet生命周期](http://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter07/verse03.html)

## 2、Filter生命周期

和Servlet生命周期类比，Filter生命周期的关键区别是：<span style="color:blue;font-weight:bold;">在Web应用启动时创建对象</span>

| 生命周期阶段 | 执行时机         | 执行次数 |
| ------------ | ---------------- | -------- |
| 创建对象     | Web应用启动时    | 一次     |
| 初始化       | 创建对象后       | 一次     |
| 拦截请求     | 接收到匹配的请求 | 多次     |
| 销毁         | Web应用卸载前    | 一次     |



[上一节](verse02.html) [回目录](index.html) [下一节](verse04.html)

