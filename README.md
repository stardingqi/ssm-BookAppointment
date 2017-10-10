# ssm-BookAppointment
优雅整合SSM框架：SpringMVC + Spring + MyBatis（用户登陆式图书预约系统）
小伙伴们开车了！
## 1、利用maven创建文件路径：
利用命令行工具输入：mvn archetype:generate -DgroupId=cn.nize  -DarchetypeArtifactId=
maven-archetype-webapp  -DarchetypeCatalog=internal
## 2、创建项目包： 
## 3、再然后就是在pom.xml里面注入依赖，maven会自动在网站上下载。关于maven大家可以看看慕课网上的教学视频。 
pom.xml
```java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>cn.nize</groupId>
  <artifactId>ssm-BookAppointment</artifactId>
  <version>1.0-SNAPSHOT</version>
  <name>ssm-BookAppointment Maven Webapp</name>
  <url>http://maven.apache.org</url>
  
	<dependencies>
		<!-- 单元测试 -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.11</version>
		</dependency>

		<!-- 1.日志 -->
		<!-- 实现slf4j接口并整合 -->
		<dependency>
			<groupId>ch.qos.logback</groupId>
			<artifactId>logback-classic</artifactId>
			<version>1.1.1</version>
		</dependency>

		<!-- 2.数据库 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.35</version>
			<!--  <scope>runtime</scope> -->
		</dependency>
		<dependency>
			<groupId>c3p0</groupId>
			<artifactId>c3p0</artifactId>
			<version>0.9.1.2</version>
		</dependency>

		<!-- DAO: MyBatis -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.3.0</version>
		</dependency>
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>1.2.3</version>
		</dependency>

		<!-- 3.Servlet web -->
		<dependency>
			<groupId>taglibs</groupId>
			<artifactId>standard</artifactId>
			<version>1.1.2</version>
		</dependency>
		<dependency>
			<groupId>jstl</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>
		<dependency>
    		<groupId>com.fasterxml.jackson.core</groupId>
    		<artifactId>jackson-databind</artifactId>
    		<version>2.5.0</version>
		</dependency>
		<dependency>
    		<groupId>com.fasterxml.jackson.core</groupId>
    		<artifactId>jackson-core</artifactId>
    		<version>2.5.0</version>
		</dependency>
		<dependency>
    		<groupId>com.fasterxml.jackson.core</groupId>
    		<artifactId>jackson-annotations</artifactId>
    		<version>2.5.0</version>
		</dependency>
	<!-- 	<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>2.5.4</version>
		</dependency> -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
		</dependency>

		<!-- 4.Spring -->
		<!-- 1)Spring核心 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>4.1.7.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-beans</artifactId>
			<version>4.1.7.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>4.1.7.RELEASE</version>
		</dependency>
		<!-- 2)Spring DAO层 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>4.1.7.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>4.1.7.RELEASE</version>
		</dependency>
		<!-- 3)Spring web -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>4.1.7.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>4.1.7.RELEASE</version>
		</dependency>
		<!-- 4)Spring test -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>4.1.7.RELEASE</version>
		</dependency>

		<!-- redis客户端:Jedis -->
		<dependency>
			<groupId>redis.clients</groupId>
			<artifactId>jedis</artifactId>
			<version>2.7.3</version>
		</dependency>
		<dependency>
			<groupId>com.dyuproject.protostuff</groupId>
			<artifactId>protostuff-core</artifactId>
			<version>1.0.8</version>
		</dependency>
		<dependency>
			<groupId>com.dyuproject.protostuff</groupId>
			<artifactId>protostuff-runtime</artifactId>
			<version>1.0.8</version>
		</dependency>

		<!-- Map工具类 -->
		<dependency>
			<groupId>commons-collections</groupId>
			<artifactId>commons-collections</artifactId>
			<version>3.2</version>
		</dependency>
	</dependencies>
 
  <build>
    <finalName>ssm-BookAppointment</finalName>
  </build>
</project>
```
## 4、做好之前的准备工作后，逻辑理顺，现在就要开始编码啦！<br> 
第一个开始填充的类当然是entiy包，他是承接我们从数据库里去除数据的类，或者把该类存入数据库，在这里我们创建两个类，一个是Book包（从数据库取出书后放入该包），一个是Appointment（存放从数据库取出的预约书籍信息）。
Book.java
```java
package com.imooc.appoint.entiy;

public class Book {
	
	private long bookId;//图书ID
	private String name;//图书名称
	private int number;//馆藏数量 
	private String introd;
	public Book() {  //为什么要有个无参构造器
	}
	public Book(long bookId, String name, int number) { 
		this.bookId = bookId;
		this.name = name;
		this.number = number;
	}
	public long getBookId() {
		return bookId;
	}
	public void setBookId(long bookId) {
		this.bookId = bookId;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getNumber() {
		return number;
	}
	public void setNumber(int number) {
		this.number = number;
	}
	public String getIntrod() {
		return introd;
	}
	public void setIntrod(String introd) {
		this.introd = introd;
	}

	@Override
	public String toString() {
		return "Book [bookId=" + bookId + ", name=" + name + ", number=" + number + ", introd=" + introd + "]";
	}
}
```
Appointment.java
```java
package com.imooc.appoint.entiy;

import java.util.Date; 

public class Appointment {
	private long bookId;
	private long studentId;
	private Date appointTime;
	// 多对一的复合属性
	private Book book;// 图书实体
	public Appointment(){
	}
	public Appointment(long bookId, long studentId, Date appointTime) { 
		this.bookId = bookId;
		this.studentId = studentId;
		this.appointTime = appointTime;
	}
	public long getBookId() {
		return bookId;
	}
	public void setBookId(long bookId) {
		this.bookId = bookId;
	}
	public long getStudentId() {
		return studentId;
	}
	public void setStudentId(long studentId) {
		this.studentId = studentId;
	}
	public Date getAppointTime() {
		return appointTime;
	}
	public void setAppointTime(Date appointTime) {
		this.appointTime = appointTime;
	}
	public Book getBook() {
		return book;
	}
	public void setBook(Book book) {
		this.book = book;
	}
	@Override
	public String toString() {
		return "Appointment [bookId=" + bookId + ", studentId=" + studentId + ", appointTime=" + appointTime + ", book="
				+ book + "]";
	}
}
```
因为我们还打算做一个学生ID、密码验证，所以也得有学生类.
Student.java
```java
package com.imooc.appoint.entiy;

public class Student {  
	private Long studentId;
	private Long password;
	
	public Student(){
		
	}
	public Student(Long studentId,Long password){
		this.studentId=studentId;
		this.password=password;
	}
	public Long getStudentId() {
		return studentId;
	}
	public void setStudentId(Long studentId) {
		this.studentId = studentId;
	}
	public Long getPassword() {
		return password;
	}
	public void setPassword(Long password) {
		this.password = password;
	}
	
	@Override
	public String toString() {
		return "Student [studentId=" + studentId + ", password=" + password + "]";
	}
}

```





