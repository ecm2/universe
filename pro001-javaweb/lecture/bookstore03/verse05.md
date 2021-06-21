[TOC]

# 第五节 前台图书展示

## 1、思路

index.html→PortalServlet.doPost()→把图书列表数据查询出来→渲染视图→页面迭代显示图书数据



## 2、实现：PortalServlet.doPost()

```java
public class PortalServlet extends ViewBaseServlet {
    
    private BookService bookService = new BookServiceImpl();

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        // 将两种请求方式的处理逻辑合并到一个方法
        doPost(request, response);
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        // 查询图书列表数据
        List<Book> bookList = bookService.getBookList();
        
        // 将图书列表数据存入请求域
        request.setAttribute("bookList", bookList);

        // 声明视图名称
        String viewName = "index";

        // 解析模板视图
        processTemplate(viewName, request, response);

    }

}
```



## 3、实现：页面迭代显示图书数据

页面文件：index.html

```html
<div class="list-content" th:if="${#lists.isEmpty(bookList)}">
    抱歉，本商城现在没有上架任何商品
</div>
<div class="list-content" th:if="${not #lists.isEmpty(bookList)}">
    <div class="list-item" th:each="book : ${bookList}">
        <img th:src="${book.imgPath}" src="static/uploads/huozhe.jpg" alt="">
        <p>书名:<span th:text="${book.bookName}">活着</span></p>
        <p>作者:<span th:text="${book.author}">余华</span></p>
        <p>价格:￥<span th:text="${book.price}">66.6</span></p>
        <p>销量:<span th:text="${book.sales}">230</span></p>
        <p>库存:<span th:text="${book.stock}">1000</span></p>
        <button>加入购物车</button>
    </div>
</div>
```



[上一节](verse04.html) [回目录](index.html)