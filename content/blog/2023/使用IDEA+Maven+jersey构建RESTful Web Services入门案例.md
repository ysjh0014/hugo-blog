---
title: "使用IDEA+Maven+jersey构建RESTful Web Services入门案例"
date: 2018-10-17 12:59:38
draft: false
---
**1.首先在WEB项目**

**2.创建好之后点击项目右键，点Add Frameworks Support给项目添加Maven框架**

**3.创建好之后的项目结构如下图所示**

![](https://img-blog.csdn.net/20181017124955617?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**4.添加pom.xml依赖**
<dependencies> <dependency> <groupId>org.glassfish.jersey.containers</groupId> <artifactId>jersey-container-servlet</artifactId> <version>2.17</version> </dependency> <dependency> <groupId>org.glassfish.jersey.media</groupId> <artifactId>jersey-media-json-jackson</artifactId> <version>2.17</version> </dependency> </dependencies>

**5.在java目录下创建POJO类User.java**

package cn.ysjh; import javax.xml.bind.annotation.XmlRootElement; @XmlRootElement public class User { private String name; private String password; private int id; public int getId() { return id; } public void setId(int id) { this.id = id; } public String getName() { return name; } public void setName(String name) { this.name = name; } public String getPassword() { return password; } public void setPassword(String password) { this.password = password; } }

**6.创建Hello类测试不同类型的返回数据**

package cn.ysjh; import javax.ws.rs.GET; import javax.ws.rs.Path; import javax.ws.rs.Produces; import javax.ws.rs.core.MediaType; @Path("/hello") public class Hello { @GET @Path("test") @Produces(MediaType.APPLICATION_XML) public User test() { User user = new User(); user.setName("zhangsan"); user.setPassword("123456789"); user.setId(12); return user; } @GET @Path("test1") @Produces(MediaType.APPLICATION_JSON) public User test1(){ User user = new User(); user.setName("zhangsan"); user.setPassword("123456789"); user.setId(13); return user; } @GET @Path("test2") @Produces(MediaType.TEXT_PLAIN) public String test2(){ return "hello world"; } }

**7.将jar包加入到WEB-INF目录下**

**8.启动Tomcat运行**

**运行结果：**

localhost:8080/api/hello/test

![](https://img-blog.csdn.net/2018101712572173?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

localhost:8080/api/hello/test1

![](https://img-blog.csdn.net/20181017125800273?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

localhost:8080/api/hello/test2

![](https://img-blog.csdn.net/20181017125840196?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)