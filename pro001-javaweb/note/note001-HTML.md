[TOC]

# HTML笔记

## 1、路径

### ①路径的分类

- 物理路径：和操作系统平台高度相关，不建议使用。
- 相对路径：基于当前资源所在的位置作为相对的基准，而将来服务器端有Java程序之后，这个相对的基准有可能是不确定的，使用相对路径很有可能出错，所以不建议使用。
- 绝对路径：以<span style="color:blue;font-weight:bold;">『服务器根目录』</span>为基准查找其他资源的路径，这个基准不会因为任何原因发生改变，所以使用绝对路径不容易出错，<span style="color:blue;font-weight:bold;">建议使用</span>。



### ②绝对路径写法

#### [1]绝对路径以斜杠开头

![images](images/img019.png)

开头的斜杠的含义：<span style="color:blue;font-weight:bold;">『服务器根目录』</span>





#### [2]在服务器根目录下查找具体Web应用

```html
<a href="/Web应用目录">超链接</a>
```



#### [3]在Web应用下查找具体资源

```html
<a href="/Web应用目录/模块目录/文件所在的目录/文件名">超链接</a>
```



## 2、HTML标签

| 标签名 | 功能 |
| -------- | ---------------------- |
| h1~h6    | 1级标题~6级标题        |
| p        | 段落                   |
| [a](note001-HTML-a.html) | 超链接                 |
| ul/li    | 无序列表               |
| [img](note001-HTML-img.html) | 图片                   |
| div      | 定义一个前后有换行的块 |
| span     | 定义一个前后无换行的块 |
| table | 定义整个表格 |
| tr | 定义表格的行 |
| [td](note001-HTML-td.html) | 定义表格行中的单元格 |
| th | 定义表格行中的表头 |
| [form](note001-HTML-form.html) | 定义整个表单 |
| [input](note001-HTML-input.html) | 定义具体表单项 |
| [button](note001-HTML-button.html) | 定义按钮 |
| [select](note001-HTML-select.html) | 定义整个下拉列表 |
| [option](note001-HTML-option.html) | 定义下拉列表中的列表项 |
| [textarea](note001-HTML-textarea.html) | 定义多行文本框 |

更多HTML标签详细信息请参照[W3School 标签列表](https://www.w3school.com.cn/tags/index.asp)



## 3、HTML实体

详细的实体符号说明，参见[W3School](https://www.w3school.com.cn/html/html_entities.asp)，常用的HTML实体参见下表：

| 符号本身 | 实体的转义符号 |
| -------- | -------------- |
| <        | \&lt;          |
| >        | \&gt;          |
| 空格     | \&nbsp;        |



[回到上一级目录](index.html)