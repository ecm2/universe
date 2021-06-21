[TOC]

# 第二节 HelloWorld

## 1、功能效果图

![images](images/img001.png)



## 2、代码实现

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>HelloWorld</title>
	</head>
	<body>
		<!-- 在HTML代码中定义一个按钮 -->
		<button type="button" id="helloBtn">SayHello</button>
	</body>
	
	<!-- 在script标签中编写JavaScript代码 -->
	<script type="text/javascript">
		
		// document对象代表整个HTML文档
		// document对象调用getElementById()方法表示根据id查找对应的元素对象
		var btnElement = document.getElementById("helloBtn");
		
		// 给按钮元素对象绑定单击响应函数
		btnElement.onclick = function(){
			
			// 弹出警告框
			alert("hello");
		};
	</script>
</html>
```



[上一节](verse01.html) [回目录](index.html) [下一节](verse03.html)