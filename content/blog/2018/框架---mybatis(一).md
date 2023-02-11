---
title: "框架---mybatis(一)"
date: 2018-03-24 13:43:44
draft: false
---
**mybatis框架介绍:**

mybatis和hibernate是平等的两个框架，都是封装好了的jdbc的持久层框架，都属于ORM(对象关系映射)框架，但是hibernate是一个完全的ORM框架，mybatis是一个不完全的ORM框架,mybatis框架原来是apache的产品，后来被github接收，现在存放在github上，下载的话可以去github上下载，地址：[github.com/mybatis](http://github.com/mybatis)

使用了mybatis可以让程序员只关注sql本身，不用去关注连接的创建，statement的创建等，mybatis会将输入参数，输出结果进行映射

为什么要用mybatis框架：

因为mybatis框架是封装的jdbc，所以这里就要说一下传统的jdbc的缺点，先附上原生jdbc的代码：

**publicstaticvoid**main(String[] args) {

Connection connection =**null**;

PreparedStatement preparedStatement =**null**;**publicstaticvoid**main(String[] args) {

Connection connection =**null**;

PreparedStatement preparedStatement =**null**;

ResultSet resultSet =**null**;

**try**{

//1、加载数据库驱动

Class.*forName*("com.mysql.jdbc.Driver");

//2、通过驱动管理类获取数据库链接

connection = DriverManager.*getConnection*("jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8","root","root");

//3、定义sql语句 ?表示占位符

String sql ="select/*from user where username=?";

/4、获取预处理statement

preparedStatement = connection.prepareStatement(sql);

//5、设置参数，第一个参数为sql语句中参数的序号（从1开始），第二个参数为设置的参数值

preparedStatement.setString(1,"王五");

//6、向数据库发出sql执行查询，查询出结果集

resultSet = preparedStatement.executeQuery();

//7、遍历查询结果集

**while**(resultSet.next()){

User user

System.*out*.println(resultSet.getString("id")+" "+resultSet.getString("username"));

}

}**catch**(Exception e) {

e.printStackTrace();

}**finally**{

//8、释放资源

**if**(resultSet!=**null**){

**try**{

resultSet.close();

}**catch**(SQLException e) {

//**TODO**Auto-generated catch block

e.printStackTrace();

}

}

**if**(preparedStatement!=**null**){

**try**{

preparedStatement.close();

}**catch**(SQLException e) {

//**TODO**Auto-generated catch block

e.printStackTrace();

}

}

**if**(connection!=**null**){

**try**{

connection.close();

}**catch**(SQLException e) {

//**TODO**Auto-generated catch block

e.printStackTrace();

}

}

}

}

jdbc的问题：

1.创建连接时，存在硬编码------配置文件(全局配置文件)

2.在执行statement时存在硬编码------配置文件(映射文件)

3.频繁的开启和关闭数据库连接，会造成数据库性能下降-------数据库连接池(全局配置文件)

**mybatis框架原理:**

**![](https://img-blog.csdn.net/20180324133826542)**