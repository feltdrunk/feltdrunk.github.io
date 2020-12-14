---
layout: post
title: MyBatis Architecture
category: JAVA
tags: [JAVA]
---

​      MyBatis是一款优秀的持久层框架，它对JDBC的操作数据的过程进行封装，使开发者只需要关注SQL本身，而不需要花费精力去处理注册驱动，创建连接，statement，设置参数，结果集检索等繁琐的代码过程。

​      MyBatis通过XML或注解将要执行的各种statement配置起来，并通过java对象和statement中的sql进行映射生成最终执行的SQL语句，最后由mybatis框架执行sql并将结果映射成java对象并返回。





## 传统jdbc访问数据库

#### 1、示例：

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class JDBCTest {
    public static void  main(String[] args) throws Exception{
        // TODO Auto-generated method stud
//      1.加载数据库驱动
        Class.forName("com.mysql.cj.jdbc.Driver");
//      2.通过驱动获得数据连接
        String url="jdbc:mysql://10.4.7.111/mybatis";
        String user="mybatis";
        String password="mybatis";
        Connection connection = DriverManager.getConnection(url,user,password);
//      3.定义SQL语句
        String sql = "select * from user where id = ?";
//      4.获取预处理的statement
        PreparedStatement  ps = connection.prepareStatement(sql);
//      5.设置参数
        ps.setInt(1,1);
//      6.执行SQL，返回结果集
        ResultSet rs = ps.executeQuery();
//      7.遍历结果集
        while (rs.next()) {
            System.out.println(rs.getInt("id") + " "+rs.getString("name"));
        }
//      8.释放资源
        rs.close();
        ps.close();
        connection.close();

    }
}
```

#### 2、问题：

Q1：数据库驱动和连接频繁的创建和释放，造成数据库资源的浪费。

 //可以使用连接池

Q2：SQL语句存在硬编码问题，不易维护。

//sql语句写在xml配置文件中

Q3：参数传递存在硬编码问题，不易维护。

//参数传递也配置在xml配置文件中

Q4：结果集中也存在硬编码问题。

//将查询结果自动的映射成java对象



……To Be Continue

​       