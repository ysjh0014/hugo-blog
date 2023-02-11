---
title: "Spring的Java配置方式"
date: 2018-11-23 18:22:58
draft: false
---
Java配置方式是Spring4.x推荐的配置方式，可以完全替代xml配置文件

Spring的Java配置方式是通过@Configuration和@Bean这两个注解实现的
@Configuration： 作用于类上，相当于一个xml配置文件
 
@Bean： 作用于方法上，相当于xml配置文件中的<bean>

下面通过一个示例来具体了解一下Spring的Java配置方式

**1.创建一个Maven工程并导入pom依赖**
<dependency> <groupId>org.springframework</groupId> <artifactId>spring-webmvc</artifactId> <version>4.3.7.RELEASE</version> </dependency>

**2.创建User对象**

package cn.ysjh.springboot.Pojo; public class User { private String username; private String password; private Integer age; public String getUsername() { return username; } public void setUsername(String username) { this.username = username; } public String getPassword() { return password; } public void setPassword(String password) { this.password = password; } public Integer getAge() { return age; } public void setAge(Integer age) { this.age = age; } }

**3.创建UserDao用于模拟数据**

package cn.ysjh.springboot.Dao; import cn.ysjh.springboot.Pojo.User; import java.util.ArrayList; import java.util.List; public class UserDao { public List<User> getUser() { ArrayList<User> result = new ArrayList<>(); User user1 = new User(); User user2 = new User(); user1.setUsername("小明"); user1.setPassword("1234"); user1.setAge(20); user2.setUsername("小强"); user2.setPassword("5678"); user2.setAge(22); result.add(user1); result.add(user2); return result; } }

**4.创建UserService用于数据操作逻辑**

package cn.ysjh.springboot.Service; import cn.ysjh.springboot.Dao.UserDao; import cn.ysjh.springboot.Pojo.User; import org.springframework.beans.factory.annotation.Autowired; import org.springframework.stereotype.Service; import java.util.List; @Service public class UserService { @Autowired private UserDao dao; public List<User> UserList(){ return dao.getUser(); } }

**5.创建SpringConfig用于实例化Spring容器**

package cn.ysjh.springboot; import cn.ysjh.springboot.Dao.UserDao; import org.springframework.context.annotation.Bean; import org.springframework.context.annotation.ComponentScan; import org.springframework.context.annotation.Configuration; @Configuration //通过该注解来表明该类是一个Spring的配置，相当于一个xml文件 @ComponentScan(basePackages = "cn.ysjh.springboot") //配置扫描包 public class SpringConfig { @Bean //通过该注解来表明是一个Bean对象，相当于xml中的<bean> public UserDao getDao(){ return new UserDao(); } }

**6.创建Test类进行测试**

package cn.ysjh.springboot; import cn.ysjh.springboot.Pojo.User; import cn.ysjh.springboot.Service.UserService; import org.springframework.boot.autoconfigure.SpringBootApplication; import org.springframework.context.annotation.AnnotationConfigApplicationContext; import java.util.List; @SpringBootApplication public class Test { public static void main(String[] args) { // 通过Java配置来实例化Spring容器 AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(SpringConfig.class); //在Spring容器中获取Bean对象 UserService service = applicationContext.getBean(UserService.class); List<User> list = service.UserList(); for (User user : list) { System.out.println(user.getUsername() + ", " + user.getPassword() + ", " + user.getAge()); } // 销毁该容器 applicationContext.destroy(); } }

**7.运行结果**

![](https://img-blog.csdnimg.cn/20181123182140428.png)

从上边的示例可以看出，使用Java配置方式可以完美的替代xml配置文件，并且结构更加清晰