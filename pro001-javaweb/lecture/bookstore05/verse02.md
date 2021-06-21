# 第二节 结账

## 1、创建订单模型

### ①物理建模

#### [1]t_order表

```sql
CREATE TABLE t_order(
	order_id INT PRIMARY KEY AUTO_INCREMENT,
	order_sequence VARCHAR(200),
	create_time VARCHAR(100),
	total_count INT,
	total_amount DOUBLE,
	order_status INT,
	user_id INT
);
```



| 字段名         | 字段作用       |
| -------------- | -------------- |
| order_id       | 主键           |
| order_sequence | 订单号         |
| create_time    | 订单创建时间   |
| total_count    | 订单的总数量   |
| total_amount   | 订单的总金额   |
| order_status   | 订单的状态     |
| user_id        | 下单的用户的id |

- 虽然order_sequence也是一个不重复的数值，但是不使用它作为主键。数据库表的主键要使用没有业务功能的字段来担任。
- 订单的状态
  - 待支付（书城项目中暂不考虑）
  - 已支付，待发货：0
  - 已发货：1
  - 确认收货：2
  - 发起退款或退货（书城项目中暂不考虑）
- 用户id
  - 从逻辑和表结构的角度来说，这其实是一个外键。
  - 但是开发过程中建议先不要加外键约束：因为开发过程中数据尚不完整，加了外键约束开发过程中使用测试数据非常不方便，建议项目预发布时添加外键约束测试。



#### [2]t_order_item表

```sql
CREATE TABLE t_order_item(
	item_id INT PRIMARY KEY AUTO_INCREMENT,
	book_name VARCHAR(20),
	price DOUBLE,
	img_path VARCHAR(50),
	item_count INT,
	item_amount DOUBLE,
	order_id VARCHAR(20)
);
```



| 字段名称    | 字段作用                     |
| ----------- | ---------------------------- |
| item_id     | 主键                         |
| book_name   | 书名                         |
| price       | 单价                         |
| item_count  | 当前订单项的数量             |
| item_amount | 当前订单项的金额             |
| order_id    | 当前订单项关联的订单表的主键 |

说明：book_name、author、price这三个字段其实属于t_book表，我们把它们加入到t_order_item表中，其实并不符合数据库设计三大范式。这里做不符合规范的操作的原因是：将这几个字段加入当前表就不必在显示数据时和t_book表做关联查询，提高查询的效率，这是一种变通的做法。



### ②逻辑模型

#### [1]Order类

```java
public class Order {

    private Integer orderId;
    private String orderSequence;
    private String createTime;
    private Integer totalCount;
    private Double totalAmount;
    private Integer orderStatus = 0;
    private Integer userId;
```



#### [2]OrdrItem类

```java
public class OrderItem {
    
    private Integer itemId;
    private String bookName;
    private Double price;
    private String imgPath;
    private Integer itemCount;
    private Double itemAmount;
    private Integer orderId;
```



## 2、创建组件

### ①持久化层

![images](images/img003.png)



### ②业务逻辑层

![images](images/img004.png)



### ③表述层

![images](images/img005.png)



## 3、结账功能

### ①具体操作清单

- 创建订单对象
- 给订单对象填充数据
  - 生成订单号
  - 生成订单的时间
  - 从购物车迁移总数量和总金额
  - 从已登录的User对象中获取userId并设置到订单对象中
- 将订单对象保存到数据库中
- 获取订单对象在数据库中自增主键的值
- 根据购物车中的CartItem集合逐个创建OrderItem对象
  - 每个OrderItem对象对应的orderId属性都使用前面获取的订单数据的自增主键的值
- 把OrderItem对象的集合保存到数据库
- 每一个item对应的图书增加销量
- 每一个item对应的图书减少库存
- 清空购物车



### ②思路

![images](images/img006.png)



### ③代码实现

#### [1]购物车页面结账超链接

cart.html

```html
<a class="pay" href="protected/OrderClientServlet?method=checkout">去结账</a>
```



#### [2]OrderClientServlet.checkout()

```java
protected void checkout(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    // 1.获取HttpSession对象
    HttpSession session = request.getSession();

    // 2.获取购物车对象
    Cart cart = (Cart) session.getAttribute("cart");
    if (cart == null) {
        String viewName = "cart/cart";

        processTemplate(viewName, request, response);

        return ;
    }

    // 3.获取已登录的用户对象
    User user = (User) session.getAttribute("user");

    // 4.调用Service方法执行结账的业务逻辑
    String orderSequence = orderService.checkout(cart, user);

    // 5.清空购物车
    session.removeAttribute("cart");

    // 6.将订单号存入请求域
    request.setAttribute("orderSequence", orderSequence);

    // 7.将页面跳转到下单成功页面
    String viewName = "cart/checkout";
    processTemplate(viewName, request, response);
}
```



#### [3]OrderService.checkout()

```java
@Override
public String checkout(Cart cart, User user) {

    // 从User对象中获取userId
    Integer userId = user.getUserId();

    // 创建订单对象
    Order order = new Order();

    // 给订单对象填充数据
    // 生成订单号=系统时间戳
    String orderSequence = System.currentTimeMillis() + "_" + userId;

    // 生成订单的时间
    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
    String createTime = simpleDateFormat.format(new Date());

    // 从购物车迁移总数量和总金额
    Integer totalCount = cart.getTotalCount();
    Double totalAmount = cart.getTotalAmount();

    order.setOrderSequence(orderSequence);
    order.setCreateTime(createTime);
    order.setTotalCount(totalCount);
    order.setTotalAmount(totalAmount);

    // 将订单对象保存到数据库中
    // ※说明：这里对insertOrder()方法的要求是获取自增的主键并将自增主键的值设置到Order对象的orderId属性中
    orderDao.insertOrder(order);

    // 获取订单对象在数据库中自增主键的值
    Integer orderId = order.getOrderId();

    // 根据购物车中的CartItem集合逐个创建OrderItem对象
    Map<String, CartItem> cartItemMap = cart.getCartItemMap();
    Collection<CartItem> cartItems = cartItemMap.values();
    List<CartItem> cartItemList = new ArrayList<>(cartItems);

    // 为了便于批量保存OrderItem，创建Object[][]
    // 二维数组第一维：SQL语句的数量
    // 二维数组第二维：SQL语句中参数的数量
    Object[][] saveOrderItemParamArr = new Object[cartItems.size()][6];

    // 为了便于批量更新Book，创建Object[][]
    Object[][] updateBookParamArr = new Object[cartItems.size()][3];

    for (int i = 0;i < cartItemList.size(); i++) {

        CartItem cartItem = cartItemList.get(i);

        // 为保存OrderItem创建Object[]
        Object[] orderItemParam = new Object[6];

        // book_name,price,img_path,item_count,item_amount,order_id
        orderItemParam[0] = cartItem.getBookName();
        orderItemParam[1] = cartItem.getPrice();
        orderItemParam[2] = cartItem.getImgPath();
        orderItemParam[3] = cartItem.getCount();
        orderItemParam[4] = cartItem.getAmount();
        orderItemParam[5] = orderId;

        // 将一维数组存入二维数组中
        saveOrderItemParamArr[i] = orderItemParam;

        // 创建数组用于保存更新Book数据的信息
        String[] bookUpdateInfoArr = new String[3];

        // 增加的销量
        bookUpdateInfoArr[0] = cartItem.getCount() + "";

        // 减少的库存
        bookUpdateInfoArr[1] = cartItem.getCount() + "";

        // bookId
        bookUpdateInfoArr[2] = cartItem.getBookId();

        // 将数组存入List集合
        updateBookParamArr[i] = bookUpdateInfoArr;
    }

    // 把OrderItem对象的集合保存到数据库：批量操作
    orderItemDao.insertOrderItemArr(saveOrderItemParamArr);

    // 使用bookUpdateInfoList对图书数据的表执行批量更新操作
    bookDao.updateBookByParamArr(updateBookParamArr);

    // 返回订单号
    return orderSequence;
}
```



#### [4]orderDao.insertOrder(order)

```java
@Override
public void insertOrder(Order order) {

    // ※DBUtils没有封装获取自增主键的方法，需要我们使用原生的JDBC来完成
    // 1.获取数据库连接
    Connection connection = JDBCUtils.getConnection();

    // 2.创建PreparedStatement对象
    String sql = "INSERT INTO t_order(order_sequence,create_time,total_count,total_amount,order_status,user_id) VALUES(?,?,?,?,?,?)";

    try {

        // ①创建PreparedStatement对象，指明需要自增的主键
        PreparedStatement preparedStatement = connection.prepareStatement(sql, PreparedStatement.RETURN_GENERATED_KEYS);

        // ②给PreparedStatement对象设置SQL语句的参数
        preparedStatement.setString(1, order.getOrderSequence());
        preparedStatement.setString(2, order.getCreateTime());
        preparedStatement.setInt(3, order.getTotalCount());
        preparedStatement.setDouble(4, order.getTotalAmount());
        preparedStatement.setInt(5, order.getOrderStatus());
        preparedStatement.setInt(6, order.getUserId());

        // ③执行更新
        preparedStatement.executeUpdate();

        // ④获取封装了自增主键的结果集
        ResultSet generatedKeysResultSet = preparedStatement.getGeneratedKeys();

        // ⑤解析结果集
        if (generatedKeysResultSet.next()) {
            int orderId = generatedKeysResultSet.getInt(1);

            order.setOrderId(orderId);
        }

    } catch (SQLException e) {
        e.printStackTrace();

        throw new RuntimeException(e);
    } finally {
        JDBCUtils.releaseConnection(connection);
    }

}
```



#### [5]BaseDao.batchUpdate()

```java
/**
 * 通用的批量增删改方法
 * @param sql
 * @param params 执行批量操作的二维数组
 *               每一条SQL语句的参数是一维数组
 *               多条SQL语句的参数就是二维数组
 * @return 每一条SQL语句返回的受影响的行数
 */
public int[] batchUpdate(String sql, Object[][] params) {

    Connection connection = JDBCUtils.getConnection();
    
    int[] rowCountArr = null;

    try {
        rowCountArr = queryRunner.batch(connection, sql, params);
    } catch (SQLException e) {
        e.printStackTrace();

        throw new RuntimeException(e);
    } finally {
        JDBCUtils.releaseConnection(connection);
    }
    
    return rowCountArr;

}
```



#### [6]orderItemDao.insertOrderItemArr(saveOrderItemParamArr)

```java
@Override
public void insertOrderItemArr(Object[][] saveOrderItemParamArr) {
    String sql = "INSERT INTO t_order_item(book_name,price,img_path,item_count,item_amount,order_id) VALUES(?,?,?,?,?,?)";

    super.batchUpdate(sql, saveOrderItemParamArr);
}
```



#### [7]bookDao.updateBookByParamArr(updateBookParamArr)

```java
@Override
public void updateBookByParamArr(Object[][] updateBookParamArr) {
    String sql = "update t_book set sales=sales+?,stock=stock-? where book_id=?";
    super.batchUpdate(sql, updateBookParamArr);
}
```



[上一节](verse01.html) [回目录](index.html) [下一节](verse03.html)