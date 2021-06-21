[TOC]

# 第九节 基本语法：${}中的表达式本质是OGNL

## 1、OGNL

OGNL：Object-Graph Navigation Language对象-图 导航语言

## 2、对象图

从根对象触发，通过特定的语法，逐层访问对象的各种属性。

![images](images/img024.png)

## 3、OGNL语法

### ①起点

在Thymeleaf环境下，${}中的表达式可以从下列元素开始：

- 访问属性域的起点
  - 请求域属性名
  - session
  - application
- param
- 内置对象
  - #request
  - #session
  - #lists
  - #strings

### ②属性访问语法

- 访问对象属性：使用getXxx()、setXxx()方法定义的属性
  - 对象.属性名
- 访问List集合或数组
  - 集合或数组[下标]
- 访问Map集合
  - Map集合.key
  - Map集合['key']



[上一节](verse08.html) [回目录](index.html) [下一节](verse10.html)