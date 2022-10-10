### MyBetis

### 1.简介

MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，

普通老式 Java 对象）为数据库中的记录

**持久层框架**

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.6</version>
</dependency>

```



### 2.第一个MyBatis程序



**思路**：搭建环境-->导入MyBatis-->编写代码-->测试

#### 编写Mybatis工具类

```java
public class Mybatisutils {

    public static SqlSessionFactory sqlSessionFactory;
    static {
        String resource = "mybatis-config.xml";
        try {
            // 使用mybatis的第一步： 获取sqlSessionFactory对象
            InputStream inputStream = Resources.getResourceAsStream(resource);
            SqlSessionFactory sqlSessionFactory = new                            
                     SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    //既然有了 SqlSessionFactory，顾名思义，
    // 我们可以从中获得 SqlSession 的实例。SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。
    public static SqlSession getSession() {
        return sqlSessionFactory.openSession();
    }

}
```

#### 编写代码

* 实体类pojo

```java
package com.kuang.pojo;

public class User {
    private int id;
    private String name;
    private String pwd;

    public User() {
    }

    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}
```

* Dao接口

```java
public interface UserDao {
    List<User> getUserList();
}
```

* 接口实现类

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace=绑定一个对应Dao/mapper接口-->
<mapper namespace="com.kuang.dao.UserDao">
    <select id="getUserList" resultType="com.kuang.pojo.User">
    select * from mybatis.user
  </select>

</mapper>
```

####  MyBatis配置文件

==数据库8.0要配置时区==

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>  // 默认事务管理器
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?       useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=UTC"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
        <environment id="test">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <mapper resource="com/kuang/dao/UserMapper.xml"/>
    </mappers>
</configuration>
```

#### pom.xml

maven资源导出失败

==maven由于他约定大于配置，我们之后可能遇到写的配置文件无法被导出或生效的问题==

如果有报(java.lang.ExceptionInInitializerError)错的话(**Maven静态资源过滤问题**)

```xml
<build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
```

### 3.CRUD

#### 1.namespace

namespace中的包名要和Dao/Mapper接口的包名一致

#### 2.select

选择、查询语句

id: 对应namespace中的方法名

resultType: 

parameterType: 参数类型

1. 编写接口

```java
 // insert 一个用户
    int addUser(User user);
```

2. 编写对应的mapper中的sql语句

```xml
  <select id="getUserList" resultType="com.kuang.pojo.User">
    select * from mybatis.user
  </select>
    <select id="getUserById" resultType="com.kuang.pojo.User" parameterType="int">
        select * from mybatis.user where id = #{id}
    </select>
```

3. 测试

```java
@Test
public void test() {
    // 1. 获取sqlSession对象
    SqlSession sqlSession = Mybatisutils.getSqlSession();
    // 2. 执行sql  方式1：getMapper
    UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
    List<User> userList = userMapper.getUserList();
    for (User user : userList) {
        System.out.println(user);
    }

    // 关闭sqlSession
    sqlSession.close();
}
```

#### 3.insert

```xml
<insert id="addUser" parameterType="com.kuang.pojo.User">
    insert into mybatis.user(id, name, pwd) values (#{id}, #{name}, #{pwd})
</insert>
```

#### 4.update

```xml
<update id="updateUser" parameterType="com.kuang.pojo.User">
    update mybatis.user set name=#{name},pwd=#{pwd} where id=#{id};
</update>
```

#### 5.delete

```xml
    <delete id="deleteUser" parameterType="com.kuang.pojo.User">
        delete from mybatis.user where id = #{id};
    </delete>
```

#### 万能map

假设，我们的实体类，或者数据库中的表，字段或者参数过多，考虑使用map

```java
int addUser2(Map<String, Object> map);
```

```xml
<!--对象中的属性可以直接拿出来     传递map的key-->
<insert id="addUser2" parameterType="map">
    insert into mybatis.user(id, name, pwd) values (#{userId}, #{userName}, #{userPwd})
</insert>
```

```java
@Test
public void addUser2() {
    SqlSession sqlSession = Mybatisutils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);

    Map<String, Object> map = new HashMap<String, Object>();
    map.put("userId", 4);
    map.put("userName", "yangwu");
    map.put("userPwd", "33553355");

    mapper.addUser2(map);

    sqlSession.close();
}
```

* Map传递参数，直接在sql中取出key即可            【parameterType="map"】

* 对象传递参数，直接在sql中取出对象的属性即可    【parameterType="Object"】

* 只有一个基本类型参数的情况下，可以直接在sql中取到

* 多个参数用map，或**注解**



### 4.配置解析

#### 1.核心配置文件

MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。 配置文档的顶层结构如下：

- configuration（配置）
  - properties（属性）
  - settings（设置）
  - typeAliases（类型别名）
  - typeHandlers（类型处理器）
  - objectFactory（对象工厂）
  - plugins（插件）
  - environments（环境配置）
    - environment（环境变量）
      - transactionManager（事务管理器）
      - dataSource（数据源）
  - databaseIdProvider（数据库厂商标识）baseIdProvider)
  - mappers（映射器）

#### 2.环境配置(environments)

MyBatis 可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

#### 3.属性(properties)

我们可以通过properties来编写一个配置文件

```properties
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=UTC
username=root
password=root
```



```xml
<properties resource="db.properties"/>  //  引入外部配置文件
```

