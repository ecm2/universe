[TOC]

# 第三节 建模

## 1、创建数据库

```sql
CREATE DATABASE bookstore210107 CHARACTER SET utf8;
USE `bookstore210107`;
```



## 2、创建数据库表

物理建模的思路[参考这里](xxx)。

```sql
CREATE TABLE t_user(
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    user_name CHAR(100),
    user_pwd CHAR(100),
    email CHAR(100)
);
```



## 3、创建Java实体类

![images](images/img011.png)



![images](images/img012.png)

```java
public class User {

    private Integer userId;// user_id
    private String userName;// user_name
    private String userPwd;// user_pwd
    private String email;// email
    ……
```



[上一节](verse02.html) [回目录](index.html) [下一节](verse04.html)