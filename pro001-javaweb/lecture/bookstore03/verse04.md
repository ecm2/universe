[TOC]

# 第四节 后台图书CRUD

> C：Create 增
>
> R：Retrieve 查
>
> U：Update 改
>
> D：Delete 删



## 1、建模

### ①物理建模

[点击这里查看SQL文件](bookstore.sql)

> 注：上面链接建议点右键→目标另存为，直接打开会显示乱码



### ②逻辑建模

```java
package com.atguigu.bookstore.entity;

public class Book {

    private Integer bookId;
    private String bookName;
    private String author;
    private Double price;
    private Integer sales;
    private Integer stock;
    private String imgPath;
```



## 2、创建并组装组件

### ①创建Servlet

<span style="color:blue;font-weight:bold;">注意</span>：由于项目分成『前台』和『后台』，所以Servlet也分成两个：

- 前台：BookPortalServlet
- 后台：BookManagerServlet



### ②创建BookService

- 接口：BookService
- 实现类：BookServiceImpl



### ③创建BookDao

- 接口：BookDao
- 实现类：BookDaoImpl



### ④组装

- 给BookManagerServlet组装BookService
- 给BookService组装BookDao



## 3、图书列表显示功能

### ①思路

manager.html→图书管理超链接→BookManagerServlet→showBookList()→book_manager.html



### ②实现：修改图书管理超链接

超链接所在文件位置：

> WEB-INF/pages/segment/admin-navigator.html

```html
<a href="BookManagerServlet?method=showBookList" class="order">图书管理</a>
```



### ③实现：BookManagerServlet.showBookList()

```java
protected void showBookList(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    // 1.调用Service方法查询图书列表
    List<Book> bookList = bookService.getBookList();
    
    // 2.将图书列表数据存入请求域
    request.setAttribute("bookList", bookList);
    
    // 3.执行视图模板渲染
    String viewName = "manager/book_manager";
    
    processTemplate(viewName, request, response);

}
```



### ④实现：BookService.getBookList()

```java
    @Override
    public List<Book> getBookList() {
        return bookDao.selectBookList();
    }
```



### ⑤实现：BookDao.selectBookList()

```java
@Override
public List<Book> selectBookList() {
    
    String sql = "select book_id bookId, book_name bookName, author, price, sales, stock, img_path imgPath from t_book order by book_Id desc";
    
    return getBeanList(Book.class, sql);
}
```



### ⑥实现：调整book_manager.html

- Thymeleaf名称空间
- base标签
- 路径中的../和./
- 包含进来的代码片段



### ⑤实现：在book_manager.html中迭代显示图书列表

```html
<tbody th:if="${#lists.isEmpty(bookList)}">
    <tr>
        <td colspan="7">抱歉，没有查询到您要的数据！</td>
    </tr>
</tbody>
<tbody th:if="${not #lists.isEmpty(bookList)}">
    <tr th:each="book : ${bookList}">
        <td>
            <img th:src="${book.imgPath}" src="static/uploads/huozhe.jpg" alt=""/>
        </td>
        <td th:text="${book.bookName}">活着</td>
        <td th:text="${book.price}">100.00</td>
        <td th:text="${book.author}">余华</td>
        <td th:text="${book.sales}">200</td>
        <td th:text="${book.stock}">400</td>
        <td>
            <a href="book_edit.html">修改</a><a href="" class="del">删除</a>
        </td>
    </tr>
</tbody>
```



## 4、图书删除功能

### ①思路

book_manager.html→删除超链接→BookManagerServlet.removeBook()→重定向显示列表功能



### ②实现：删除超链接

```html
<a th:href="@{/BookManagerServlet(method=removeBook,bookId=${book.bookId})}" class="del">删除</a>
```



### ③实现：BookManagerServlet.removeBook()

```java
protected void removeBook(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
   
    // 1.从请求参数中获取bookId
    String bookId = request.getParameter("bookId");
    
    // 2.调用Service方法执行删除
    bookService.removeBook(bookId);
    
    // 3.重定向到显示列表功能
    response.sendRedirect(request.getContextPath() + "/BookManagerServlet?method=showBookList");

}
```



### ④实现：BookService.removeBook()

```java
    @Override
    public void removeBook(String bookId) {
        bookDao.deleteBook(bookId);
    }
```



### ⑤实现：BookDao.deleteBook()

```java
    @Override
    public void deleteBook(String bookId) {
        String sql = "delete from t_book where book_id=?";
        
        update(sql, bookId);
    }
```



## 5、新增图书功能

### ①思路

book_manager.html→添加图书超链接→BookManagerServlet.toAddPage()→book_add.html

book_add.html→提交表单→BookManagerServlet.saveBook()→重定向显示列表功能



### ②实现：添加图书超链接

```html
<a href="BookManagerServlet?method=toAddPage">添加图书</a>
```



### ③实现：BookManagerServlet.toAddPage()

```html
protected void toAddPage(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    
    String viewName = "book_add";
    
    processTemplate(viewName, request, response);
    
}
```



### ④实现：book_add.html

由book_edit.html复制出来，然后调整表单标签：

```html
<form action="BookManagerServlet" method="post">
    <input type="hidden" name="method" value="saveBook" />
    <input type="text" name="bookName" placeholder="请输入名称"/>
    <input type="number" name="price" placeholder="请输入价格"/>
    <input type="text" name="author" placeholder="请输入作者"/>
    <input type="number" name="sales" placeholder="请输入销量"/>
    <input type="number" name="stock" placeholder="请输入库存"/>
```



### ⑤实现：BookManagerServlet.saveBook()

```java
protected void saveBook(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    // 1.获取请求参数
    String bookName = request.getParameter("bookName");
    String price = request.getParameter("price");
    String author = request.getParameter("author");
    String sales = request.getParameter("sales");
    String stock = request.getParameter("stock");

    // 2.封装对象

    // ※imgPath按说应该通过文件上传的方式提供，但是现在这个技术还没学
    // 所以暂时使用一个固定值
    String imgPath = "static/uploads/mi.jpg";

    Book book = new Book(null, bookName, author, Double.parseDouble(price), Integer.parseInt(sales), Integer.parseInt(stock), imgPath);

    // 3.调用Service方法执行保存
    bookService.saveBook(book);

    // 4.重定向到显示列表页面
    response.sendRedirect(request.getContextPath() + "/BookManagerServlet?method=showBookList");
}
```



### ⑥实现：BookService.saveBook()

```java
    @Override
    public void saveBook(Book book) {
        bookDao.insertBook(book);
    }
```



### ⑦实现：BookDao.insertBook()

```java
@Override
public void insertBook(Book book) {
    String sql = "insert  into `t_book`(`book_name`,`author`,`price`,`sales`,`stock`,`img_path`) values (?,?,?,?,?,?)";

    update(sql, book.getBookName(), book.getAuthor(), book.getPrice(), book.getSales(), book.getStock(), book.getImgPath());
}
```



## 6、修改图书功能

### ①思路

book_manager.html→修改图书超链接→BookManagerServlet.toEditPage()→book_edit.html（表单回显）

book_edit.html→提交表单→BookManagerServlet.updateBook()→重定向显示列表功能



### ②实现：修改图书超链接

```html
<a th:href="@{/BookManagerServlet(method=toEditPage,bookId=${book.bookId})}">删除</a>
```



### ③实现：BookManagerServlet.toEditPage()

```java
protected void toEditPage(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    
    // 1.从请求参数中获取bookId
    String bookId = request.getParameter("bookId");
    
    // 2.根据bookId查询对应的Book对象
    Book book = bookService.getBookById(bookId);
    
    // 3.把Book对象存入请求域
    request.setAttribute("book", book);
    
    // 4.渲染视图
    String viewName = "manager/book_edit";
    
    processTemplate(viewName, request, response);

}
```



### ④实现：BookService.getBookById()

```java
    @Override
    public Book getBookById(String bookId) {
        return bookDao.selectBookByPrimaryKey(bookId);
    }
```



### ⑤实现：BookDao.selectBookByPrimaryKey()

```java
@Override
public Book selectBookByPrimaryKey(String bookId) {
    
    String sql = "select book_id bookId, book_name bookName, author, price, sales, stock, img_path imgPath from t_book where book_id=?";
    
    return getBean(Book.class, sql, bookId);
}
```



### ⑥实现：book_edit.html（表单回显）

```html
<form action="BookManagerServlet" method="post">
    <input type="hidden" name="method" value="updateBook" />
    <input type="text" name="bookName" th:value="${book.bookName}" placeholder="请输入名称"/>
    <input type="number" name="price" th:value="${book.price}" placeholder="请输入价格"/>
    <input type="text" name="author" th:value="${book.author}" placeholder="请输入作者"/>
    <input type="number" name="sales" th:value="${book.sales}" placeholder="请输入销量"/>
    <input type="number" name="stock" th:value="${book.stock}" placeholder="请输入库存"/>
    
    <button class="btn">更新</button>
```



### ⑦实现：BookManagerServlet.updateBook()

```java
protected void updateBook(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    
    // 1.获取请求参数
    String bookId = request.getParameter("bookId");
    String bookName = request.getParameter("bookName");
    String price = request.getParameter("price");
    String author = request.getParameter("author");
    String sales = request.getParameter("sales");
    String stock = request.getParameter("stock");
    
    // 2.封装对象
    Book book = new Book(Integer.parseInt(bookId), bookName, author, Double.parseDouble(price), Integer.parseInt(sales), Integer.parseInt(stock), null);

    // 3.调用Service方法执行更新
    bookService.updateBook(book);
    
    // 4.渲染视图
    response.sendRedirect(request.getContextPath() + "/BookManagerServlet?method=showBookList");
}
```



### ⑧实现：BookService.updateBook()

```java
    @Override
    public void updateBook(Book book) {
        bookDao.updateBook(book);
    }
```



### ⑨实现：BookDao.updateBook()

<span style="color:blue;font-weight:bold;">注意</span>：这里不修改imgPath字段

```java
@Override
public void updateBook(Book book) {
    String sql = "update t_book set book_name=?, author=?, price=?, sales=?, stock=? where book_id=?";
    
    update(sql, book.getBookName(), book.getAuthor(), book.getPrice(), book.getSales(), book.getStock(), book.getBookId());
}
```



[上一节](verse03.html) [回目录](index.html) [下一节](verse05.html)