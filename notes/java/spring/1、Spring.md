### 1、Spring

#### 1.1 

Spring 以interface21 为基础

2004年3.24诞生

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.0.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.0.RELEASE</version>
</dependency>

```

#### 1.2

**优点**：开源的免费的容器； 

 			轻量级的、非入侵式的框架；

​			控制反转（IOC），面向切面编程（AOP）

​			支持事务处理，，对框架整合支持

### 2、IOC理论推导

我们使用一个Set接口实现，已经发生了革命性的变化！

```java
    private UserDao userDao;
    // 利用set进行动态实现值的注入
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

```

+ 之前，程序是主动创建对象，控制权在程序员手上
+ 使用了set之后，程序不在具有主动性，而是变成了被动的接受对象

这种思想从本质上，解决了问题，我们程序员不用再去管理对象的创建了。系统的耦合性大大降低，可以更加专注的在业务的实现上！

