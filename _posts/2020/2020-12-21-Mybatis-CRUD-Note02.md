---
layout: post
title: Mybatis CRUD
category: Mybatis
tags: [Mybatis]
---

Quick note about MyBatis CRUD.



​                                                                                                                                                  ![MyBatis logo](https://mybatis.org/images/mybatis-logo.png)


## 1.CRUD

### 1.1 namespace

namespace中的包名要和Dao/mapper接口的包名一致！

### 1.2 select

选择、查询语句：

- id：就是对应namespace中的方法名：
- resultType：Sql语句执行的返回值
- parameterType： 参数类型

1. 编写接口

   ```xml
       //根据ID查询用户
       User getUserbyId(int id);
   ```

2. 编写对应的mapper中的sql语句

   ```xml
      <select id="getUserbyId" parameterType="int" resultType="com.stone.pojo.User">
           select * from mybatis.user where id = #{id}
       </select>
   ```

3. 测试

   ```java
      @Test
       public void getUserById(){
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           User user = mapper.getUserbyId(1);
   
           System.out.println(user);
   
           sqlSession.close();
       }
   ```

### 1.3 Insert

 ```xml
    <insert id="addUser" parameterType="com.stone.pojo.User">
        insert into mybatis.user (ID,NAME,PWD) values (#{ID},#{NAME},#{PWD});
    </insert>
 ```

### 1.4 Update

```xml
    <update id="updateUser" parameterType="com.stone.pojo.User">
        update mybatis.user set NAME = #{NAME},PWD = #{PWD} where ID = #{ID};
    </update>
```

### 1.5 Delete

```xml
   <delete id="deleteUser" parameterType="int">
        delete from mybatis.user where ID = #{ID}
    </delete>
```

注意点：

- 增删该需要提交事务！

### 1.6 错误分析

- 标签不要匹配错
- resource 绑定mapper，需要使用路径
- 程序配置文件必须符合规范
- NullPointerException，没有注册到资源
- 输出的xml文件中存在中文乱码问题
- maven资源没有导出问题

### 1.7 万能的map

假设，我们的实体类，或者数据库中的表，字段或者参数过多，我们应当考虑使用Map

        ```xml
    //万能的MAP
    int addUser2(Map<String,Object> map);
        ```

```xml
<insert id="addUser2" parameterType="map">
    insert into mybatis.user(ID,NAME) values (#{userid},#{username})
</insert>
```

```java
@Test
public void addUser2(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);

    HashMap<String, Object> map = new HashMap<>();
    map.put("userid",5);
    map.put("username","呵呵");

    mapper.addUser2(map);

    sqlSession.commit();
    sqlSession.close();
}
```

Map传递参数，直接在sql中取出key即可！【parameterType="map"】

对象传递参数，直接在sql中取对象的属性即可！【parameterType="object"】

只有一个基本类型参数的情况下，可以直接在sql中取到！

多个参数用Map，**或注解**。

### 1.8 思考题

模糊查询怎么写

1.java代码执行的时候，传递通配符% %

```java
 List<User> userlist = mapper.getUserLike("%李%");
```

2.在sql拼接中使用通配符！

```sql
 select * from mybatis.user where name like "%"#{value}"%"
```



## 2.配置解析

### 2.1 核心配置文件

- mybatis-config.xml

- MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。

  ```xml
  configuration（配置）
  properties（属性）
  settings（设置）
  typeAliases（类型别名）
  typeHandlers（类型处理器）
  objectFactory（对象工厂）
  plugins（插件）
  environments（环境配置）
  environment（环境变量）
  transactionManager（事务管理器）
  dataSource（数据源）
  databaseIdProvider（数据库厂商标识）
  mappers（映射器）
  ```



### 2.2 环境配置（environments）

Mybatis可以配置成适应多种环境

不过要记住：尽管可以配置多个环境，但每个SqlSessionFactory实例只能选择一种环境。

学会使用配置多套

Mybatis默认的事务管理器就是JDBC，连接池：POOLED

### 2.3 属性（properties）

我们可以通过properties属性来实现引用配置文件

![image-20201220232555803](https://feltdrunk.github.io/img/image-20201220232420317.png)

这些属性是可外部配置且可动态替换的，既可以在典型的Java属性文件中配置，亦可以通过properties 元素的子元素来传递。

```xml
<properties resource="db.properties">
    <property name="username" value="mybatis"/>
    <property name="password" value="mybatis"/>
</properties>
```

【db.properties】

```properties
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://10.4.7.111:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8
username=mybatis
password=mybatis
```

- 可以直接引入外部文件

- 可以在其中增加一些属性配置

- 如果两个文件有同一个字段，优先使用外部配置文件的

  

### 2.4 类型别名（typeAliases）

- 类型别名可为 Java 类型设置一个缩写名字。
- 它仅用于 XML 配置，意在降低冗余的全限定类名书写。

```xml
<!--可以给实体类起别名-->
    <typeAliases>
        <typeAlias type="com.stone.pojo.User" alias="User"/>
    </typeAliases>
```

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean。比如：扫描实体类的包，它的默认别名就为这个类的类名，首字母小写！

```xml
<!--可以给实体类起别名-->
    <typeAliases>
        <package name="com.stone.pojo"/>
    </typeAliases>
```

在实体类比较少的时候，使用第一种方式。

如果实体类十分多，建议使用第二种。

第一种可以DIY别名，第二种则不行，如果非要改，需要在实体上增加注解

```java
import org.apache.ibatis.type.Alias;

//实体类
@Alias("user")
public class User {}
```

### 2.5 设置（settings）

| 设置名             | 描述                                                         | 有效值                                                       | 默认值 |
| :----------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :----- |
| cacheEnabled       | 全局性地开启或关闭所有映射器配置文件中已配置的任何缓存。     | true \| false                                                | true   |
| lazyLoadingEnabled | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 `fetchType` 属性来覆盖该项的开关状态。 | true \| false                                                | false  |
| logImpl            | 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。        | SLF4J \| LOG4J \| LOG4J2 \| JDK_LOGGING \| COMMONS_LOGGING \| STDOUT_LOGGING \| NO_LOGGING | 未设置 |

### 2.6 其他配置

- [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)

- [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)

- [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)

  - mybatis-generator-core

  - mybatis-plus

  - 通用mapper

    

### 2.7  映射器（mappers）

MapperRegistry：注册绑定我们的Mapper文件

方式一：

```xml
    <!--每一个Mapper.xml都需要在Mybatis核心配置文件中注册-->
    <mappers>
        <mapper resource="com/stone/dao/UserMapper.xml"/>
    </mappers>
```



方式二：使用class文件绑定注册

```xml
    <mappers>
        <mapper class="com.stone.dao.UserMapper"/>
    </mappers>
```

注意点：

- 接口和他的Mapper配置文件必须同名且在同一个包下！

  

方式三：使用扫描包进行注入绑定

```xml
  <mappers>
        <package name="com.stone.dao"/>
    </mappers>
```

注意点：

- 接口和他的Mapper配置文件必须同名且在同一个包下！



### 2.8作用域（Scope）和生命周期

理解我们之前讨论过的不同作用域和生命周期类别是至关重要的，因为错误的使用会导致非常严重的并发问。

#### SqlSessionFactoryBuilder

- 一旦创建了 SqlSessionFactory，就不再需要它了
- 局部变量

#### SqlSessionFactory

- 说白了就是可以想象为：数据库连接池
- SqlSessionFactory 一旦被创建就应该在运行期间一直存在，没有任何理由丢弃它或重新创建另一个实例。
- SqlSessionFactory 的最佳作用域是应用作用域。
- 最简单的就是使用单例模式或者静态单例模式。

#### SqlSession

- 连接到连接池的一个请求
- SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。
- 用完后需要赶紧关闭，否则资源被占用！

![image-20201221005126561](https://feltdrunk.github.io/img/image-20201221004708294.png)
