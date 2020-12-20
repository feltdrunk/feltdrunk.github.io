---
layout: post
title: Mybatis Note01
category: Mybatis
tags: [Mybatis]
---

Quick note about MyBatis configuration.



​                                                                                                                                                  ![MyBatis logo](https://mybatis.org/images/mybatis-logo.png)

## 简介

### 什么是Mybatis

- MyBatis 是一款优秀的持久层框架，
- 它支持自定义 SQL、存储过程以及高级映射。
- MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。
- MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
- Mybatis本是apache的一个开源项目iBatis，2010年这个项目由apache software foundation 迁移到了google code，并改名为MyBatis
- 2013年11月迁移到Github



如何获取Mybatis

- mvaen仓库：

  ```xml
  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.2</version>
  </dependency>
  ```

- Github： [https://github.com/mybatis/mybatis-3](https://github.com/mybatis/mybatis-3)

- 中文文档：[https://mybatis.org/mybatis-3/zh/index.html](https://mybatis.org/mybatis-3/zh/index.html)

###  持久化

数据持久化

- 持久化就是将程序的数据在持久状态和瞬时状态转化的过程。
- 内存： **断电即失**
- 生活：冷藏，罐头。

为什么需要持久化？

- 有一些对象，不能让他丢掉。
- 内存太贵

### 持久层

Dao层，Service层，Controller层

-  完成持久化工作的代码块。
- 层界限十分明显。

### 为什么需要Mybatis

- 帮助程序员将数据存入到数据库中
- 方便
- 传统的JDBC代码太复杂了，简化。框架。

优点：

- 简单易学：
- 灵活：
- 解除sql与程序代码的耦合：通过提供DAO层，将业务逻辑和数据访问逻辑分离，使系统的设计更清晰，更易维护，更易单元测试。sql和代码的分离，提高了可维护性。
- 提供映射标签，支持对象与数据库的orm字段关系映射
- 提供对象关系映射标签，支持对象关系组建维护
- 提供xml标签，支持编写动态sql



## 第一个MyBatis程序

思路：搭建环境-->导入Mybatis-->编写代码-->测试

### 搭建环境

- 搭建数据库

  ```mysql
  CREATE DATABASE `mybatis`;
  USE `mybatis`;
  
  CREATE TABLE `user`(
      `id` INT(20) NOT NULL PRIMARY KEY,
      `name` VARCHAR(30) DEFAULT NULL,
      `pwd` VARCHAR(30) DEFAULT NULL
  )ENGINE=INNODB DEFAULT CHARSET=utf8;
  
  INSERT INTO `user`(`id`,`name`,`pwd`) VALUES
  (1,'张三','123456'),
  (2,'李四','123456'),
  (3,'王五','123456')
  ```

- 新建项目

- 普通maven项目

- 删除src目录

- 导入maven依赖

  ```xml
  <!-- 导入依赖 -->
      <dependencies>
          <!--mysql驱动-->
          <dependency>
              <groupId>mysql</groupId>
              <artifactId>mysql-connector-java</artifactId>
              <version>5.1.47</version>
          </dependency>
          <!--mybatis驱动-->
          <dependency>
              <groupId>org.mybatis</groupId>
              <artifactId>mybatis</artifactId>
              <version>3.5.2</version>
          </dependency>
          <!--junit驱动-->
          <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>4.12</version>
          </dependency>
      </dependencies>
  ```



### 创建模块

- 编写mybatis的核心配置文件

  ```xml
  <!--核心配置文件-->
  <configuration>
      <environments default="development">
          <environment id="development">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
                  <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;charaterEncoding=UTF-8"/>
                  <property name="username" value="mybatis"/>
                  <property name="password" value="mybatis"/>
              </dataSource>
          </environment>
      </environments>
      
      <!--每一个Mapper.xml都需要在Mybatis核心配置文件中注册-->
      <mappers>
          <mapper resource="com/stone/dao/UserMapper.xml"/>
      </mappers>
  
  </configuration>
  ```

  

- 编写mybatis的工具类

  ```java
  //sqlSessionFactory --> sqlSession
  public class MybatisUtils {
  
      private static SqlSessionFactory sqlSessionFactory;
      static{
          try {
              //使用mybatis第一步：获取sqlSessionFactory对象
              String resource = "mybatis-config.xml";
              InputStream inputStream = Resources.getResourceAsStream(resource);
              sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
          } catch (Exception e) {
              e.printStackTrace();
          }
      }
      //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。
      // SqlSession 提供了在数据库执行 SQL 命令所需的所有方法
  
      public static SqlSession getSqlSession(){
          return sqlSessionFactory.openSession();
      }
  }
  ```



### 编写代码

- 实体类

  ```java
  //实体类
  public class User {
      private int ID;
      private String NAME;
      private String PWD;
  
      public User() {
      }
  
      public User(int ID, String NAME, String PWD) {
          this.ID = ID;
          this.NAME = NAME;
          this.PWD = PWD;
      }
  
      public int getID() {
          return ID;
      }
  
      public void setID(int ID) {
          this.ID = ID;
      }
  
      public String getNAME() {
          return NAME;
      }
  
      public void setNAME(String NAME) {
          this.NAME = NAME;
      }
  
      public String getPWD() {
          return PWD;
      }
  
      public void setPWD(String PWD) {
          this.PWD = PWD;
      }
  
      @Override
      public String toString() {
          return "User{" +
                  "ID=" + ID +
                  ", NAME='" + NAME + '\'' +
                  ", PWD='" + PWD + '\'' +
                  '}';
      }
  }
  ```

  

- Dao接口

  ```java
  public interface UserDao {
      List<User> getUserList();
  }
  ```

  

- 接口实现类由原来的UserDaoImpl转变为一个Mapper配置文件

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <!--namespace=绑定一个对应的Dao/Mapper接口-->
  <mapper namespace="com.stone.dao.UserDao">
      <!--select查询语句-->
      <select id="getUserList" resultType="com.stone.pojo.User">
          select * from mybatis.user
      </select>
  
  </mapper>
  ```



###  测试

- junit测试

  ```java
  public class UserDaoTest {
      @Test
      public void test(){
          //第一步：获取SqlSeesion对象
          SqlSession sqlSession = MybatisUtils.getSqlSession();
          //第二步 getMapper
          UserDao userDao = sqlSession.getMapper(UserDao.class);
          List<User> userlist = userDao.getUserList();
  
          for (User user : userlist){
              System.out.println(user);
          }
          //关闭SqlSession
          sqlSession.close();
      }
  }  
  ```

- 可能会遇到的问题：

   1、配置文件没有注册

   2、绑定接口错误

   3、方法名不对

   4、返回类型不对

   5、Maven导出资源问题

  ```xml
  <build>
      <resources>
          <resource>
              <directory>src/main/java</directory>
              <includes>
                  <include>**/*.properties</include>
                  <include>**/*.xml</include>
              </includes>
              <filtering>ture</filtering>
          </resource>
          <resource>
              <directory>src/main/resources</directory>
              <includes>
                  <include>**/*.properties</include>
                  <include>**/*.xml</include>
              </includes>
              <filtering>ture</filtering>
          </resource>
      </resources>
  </build> 
  ```

  





​     





​                

​             

​            





