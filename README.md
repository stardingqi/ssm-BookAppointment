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
## 4、做好之前的准备工作后，逻辑理顺，现在就要开始编码啦，首先我们创建数据库！<br> 
根据我们的实际情况，在数据库中创建表：<br>
schema.sql
```java
-- 创建图书表
CREATE TABLE `book` ( 
  `book_id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '图书ID',
  `name` varchar(100) NOT NULL COMMENT '图书名称',
  `introd` varchar(1000) NOT NULL COMMENT '简介',
  `number` int(11) NOT NULL COMMENT '馆藏数量',
  PRIMARY KEY (`book_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1000 DEFAULT CHARSET=utf8 COMMENT='图书表';

-- 初始化图书数据
INSERT INTO `book` (`book_id`, `name`, `introd`,`number`)
VALUES
	(1000, 'Java程序设计', 'Java程序设计是一门balbal',10),
	(1001, '数据结构','数据结构是计算机存储、组织数据的方式。数据结构是指相互之间存在一种或多种特定关系的数据元素的集合。', 10),
	(1002, '设计模式','设计模式（Design Pattern）是一套被反复使用、多数人知晓的、经过分类的、代码设计经验的总结。',10),
	(1003, '编译原理','编译原理是计算机专业的一门重要专业课，旨在介绍编译程序构造的一般原理和基本方法。',10),
	(1004,'大学语文','基于长期的教学实践和学科思考，我们编写了这本《大学语文》教材balbal',10),
	(1005,'计算方法','计算方法又称“数值分析”。是为各种数学问题的数值解答研究提供最有效的算法。',10),
	(1006,'高等数学','广义地说，初等数学之外的数学都是高等数学，也有将中学较深入的代数、几何以及简单的集合论初步balbal',10);

-- 创建预约图书表
CREATE TABLE `appointment` (
  `book_id` bigint(20) NOT NULL COMMENT '图书ID',
  `student_id` bigint(20) NOT NULL COMMENT '学号',
  `appoint_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '预约时间' ,
  PRIMARY KEY (`book_id`, `student_id`),
  INDEX `idx_appoint_time` (`appoint_time`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='预约图书表';
-- 创建学生数据表
CREATE TABLE `student`(
	`student_id` bigint(20) NOT NULL COMMENT '学生ID',
	`password`  bigint(20) NOT NULL COMMENT '密码',
	PRIMARY KEY(`student_id`)
)ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='学生统计表';	
-- 初始化学生数据
INSERT INTO `student`(`student_id`,`password`) 
VALUES
	(3211200801,346543),
	(3211200802,754323),
	(3211200803,674554),
	(3211200804,542344),
	(3211200805,298383),
	(3211200806,873973),
	(3211200807,193737),
	(3211200808,873092);
	
```
## 5 根据数据库对象创建实体类。<br>
第一个开始填充的类当然是entiy包，他是承接我们从数据库里去除数据的类，或者把该类存入数据库，在这里我们创建两个类，一个是Book包（从数据库取出书后放入该包），一个是Appointment（存放从数据库取出的预约书籍信息）。<br>
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
因为我们还打算做一个学生ID、密码验证，所以也得有学生类.<br>
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
## 6创建DAO中的类<br>
DAO包中的类主要功能是实现与数据库交互，所以我们需要大的逻辑是：1、学生类与数据库交互；2、预约是与数据库交互；3、查询书时与时与数据库交互。这里大致分为几类，等具体写接口时再去细写。还有就是注意，DAO只是与数据库交互，不能写交互组成的逻辑，这个放在service里面。我们应该站在用户的角度设计DAO接口，至于具体怎么用，后面再写。<br>
BookDao.java
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
AppointmentDao.java
```java
package com.imooc.appoint.dao;

import java.util.List;

import org.apache.ibatis.annotations.Param;

import com.imooc.appoint.entiy.Appointment; 

public interface AppointmentDao {
	//通过图书ID和学生ID预约书籍，并插入
	int insertAppointment(@Param("bookId") long bookId, @Param("studentId") long studentId);
	 
	//通过一个学生ID查询已经预约了哪些书。
	List<Appointment> quaryAndReturn(long studentId);
 
}

```
StudentDao.java
```java
package com.imooc.appoint.dao;

import org.apache.ibatis.annotations.Param;

import com.imooc.appoint.entiy.Student;

public interface StudentDao {
	/**
	 * 向数据库验证输入的密码是否正确
	 */
	Student quaryStudent(@Param("studentId") long studentId, @Param("password") long password);
}

```
## 7我们暂时先不管以上接口实现，先为DAO配置入框架，实现spring与mybatis的整合。<br>
 因为spring的配置太多，我们这里分三层，分别是dao service web。我们这里先写spring-dao.xml，其他的等我们实现相应包后再去配置。
 配置时，主要注意一下几个方面：<br>
1、读入数据库连接相关参数（可选）<br>
2、配置数据连接池<br>
3、配置连接属性，可以不读配置项文件直接在这里写死<br>
4、配置c3p0，只配了几个常用的<br>
5、配置SqlSessionFactory对象（mybatis）<br>
6、扫描dao层接口，动态实现dao接口，也就是说不需要daoImpl，sql和参数都写在xml文件上<br>
spring-dao.xml
```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd">
			<!-- 配置整合mybatis过程 -->
	<!-- 1.配置数据库相关参数   properties的属性：${url} -->
	<context:property-placeholder location="classpath:jdbc.properties" />

	<!-- 2.数据库连接池 -->
	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<!-- 配置连接池属性 -->
		<property name="driverClass" value="${jdbc.driver}" />
		<property name="jdbcUrl" value="${jdbc.url}" />
		<property name="user" value="${jdbc.username}" />
		<property name="password" value="${jdbc.password}" />

		<!-- c3p0连接池的私有属性 -->
		<property name="maxPoolSize" value="30" />
		<property name="minPoolSize" value="10" />
		<!-- 关闭连接后不自动commit -->
		<property name="autoCommitOnClose" value="false" />
		<!-- 获取连接超时时间 -->
		<property name="checkoutTimeout" value="10000" />
		<!-- 当获取连接失败重试次数 -->
		<property name="acquireRetryAttempts" value="2" />
	</bean>
		<!-- 3.配置SqlSessionFactory对象 -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<!-- 注入数据库连接池 -->
		<property name="dataSource" ref="dataSource" />
		<!-- 配置MyBaties全局配置文件:mybatis-config.xml -->
		<property name="configLocation" value="classpath:mybatis-config.xml" />
		<!-- 扫描entity包 使用别名 -->
		<property name="typeAliasesPackage" value="com.imooc.appoint.entity" /> 
		<!-- 扫描sql配置文件:mapper需要的xml文件 -->
		<property name="mapperLocations" value="classpath:mapper/*.xml" />
	</bean>

	<!-- 4.配置扫描Dao接口包，动态实现Dao接口，注入到spring容器中 -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<!-- 注入sqlSessionFactory -->
		<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
		<!-- 给出需要扫描Dao接口包 -->
		<property name="basePackage" value="com.imooc.appoint.dao" /> 
	</bean>
	
</beans>
```
### 配置jdbc.properties文件<br>
```java
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://127.0.0.1:3306/ssmbookappoint?useUnicode=true&characterEncoding=utf8
jdbc.username= 
jdbc.password= 
```
第一次上传忘记去掉用户名密码了，好险！大家用的时候填上自己的用户名、密码。<br>
### 配置mybatis。<br>
所以需要配置mybatis核心文件，在recources文件夹里新建mybatis-config.xml文件。<br>

1、使用自增主键<br>
2、使用列别名<br>
3、开启驼峰命名转换 create_time -> createTime<br>
mybatis-config.xml
```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 配置全局属性 -->
	<settings>
		<!-- 使用jdbc的getGeneratedKeys获取数据库自增主键值 -->
		<setting name="useGeneratedKeys" value="true" />

		<!-- 使用列别名替换列名 默认:true -->
		<setting name="useColumnLabel" value="true" />

		<!-- 开启驼峰命名转换:Table{create_time} -> Entity{createTime} -->
		<setting name="mapUnderscoreToCamelCase" value="true" />
	</settings>
</configuration>
```
### 配置日志记录文件logback.xml <br>
因为在运行时，我们经常会查看日志什么的，所以我们把这个配置进去<br>
logback.xml
```java
<?xml version="1.0" encoding="UTF-8"?><!-- 我们在项目中经常会使用到日志，所以这里还有配置日志xml -->
<configuration debug="true">
	<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
		<!-- encoders are by default assigned the type ch.qos.logback.classic.encoder.PatternLayoutEncoder -->
		<encoder>
			<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
		</encoder>
	</appender>

	<root level="debug">
		<appender-ref ref="STDOUT" />
	</root>
</configuration>
```
## 8配置实现接口的xml<br>
我们要开始写DAO接口的实现类，因为我们配置扫描sql配置文件路径是:mapper下的xml，所有我们应该在resourse下新建mapper文件夹，在这里存放实现DAO接口的各种xml<br>
BookDao.xml
```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.imooc.appoint.dao.BookDao">
	<select id="queryById" parameterType="long" resultType="com.imooc.appoint.entiy.Book" >
		<!-- 具体的sql -->
		SELECT
			book_id,
			name,
			introd,
			number
		FROM
			book
		WHERE
			book_id = #{bookId}
	</select>

	<select id="querySome" parameterType="com.imooc.appoint.entiy.Book" resultType="com.imooc.appoint.entiy.Book">
		SELECT book_id,name,introd,number FROM book
		<where>
			<!-- <if test="name !=null and !&quot;&quot;.equals(name.trim())">  -->
				and name like '%' #{name} '%'
			<!--  </if>   -->
		</where> 
	</select> 
		
	<select id="queryAll" resultType="com.imooc.appoint.entiy.Book">
		SELECT
			book_id,
			name,
			introd,
			number
		FROM
			book
		ORDER BY
			book_id
		LIMIT #{offset}, #{limit}
	</select>
	
	<update id="reduceNumber">
		UPDATE book
		SET number = number - 1
		WHERE
			book_id = #{bookId}
		AND number > 0
	</update>

</mapper>
```
AppointmentDao.xml
```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<mapper namespace="com.imooc.appoint.dao.AppointmentDao">
    <insert id="insertAppointment">
		<!-- ignore 主键冲突，报错 -->
		INSERT ignore INTO appointment (book_id, student_id)
		VALUES (#{bookId}, #{studentId})
	</insert>
<!--     <delete id="deleteOne" >
    	DELETE FROM appointment WHERE book_id=#{bookId} AND student_id=#{studentId} 
    </delete>  用不到这个功能-->
    <!-- //查询某个学生的所有预约记录 -->
    <select id="quaryAndReturn"  resultType="com.imooc.appoint.entiy.Appointment">
		<!-- 如何告诉MyBatis把结果映射到Appointment同时映射book属性 -->
		<!-- 可以自由控制SQL -->
		SELECT
			a.book_id,<!--mybatis会认为是book_id,又因为开启了驼峰命名法 所以最终是bookId -->
			a.student_id,
			a.appoint_time,
			b.book_id "book.book_id",<!--b.book_id as "book.book_id" 告诉mybatis b.book_id是Appointment中book属性的值-->
			b.`name` "book.name",
			b.introd "book.introd",
			b.number "book.number"
		FROM
			appointment a
		INNER JOIN book b ON a.book_id = b.book_id
		WHERE 
		 	a.student_id = #{studentId}
	</select>
<!-- 	//查询所有被预约了的书
	<select id="queryAll" >
		SELECT 
			a.book_id, 
			a.student_id,
			a.appoint_time, 
			b.book_id "book.book_id", 
			b.`name` "book.name",
			b.introd "book.introd"
			b.number "book.number"
		FROM 
			appointment a
		INNER JOIN book b ON a.book_id = b.book_id	
		ORDER BY
			a.book_id
		LIMIT #{offset}, #{limit}
	</select> 暂时用不到这个功能，管理员界面待开发-->
	
</mapper>
```
StudentDao.java
```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.imooc.appoint.dao.StudentDao">
	<select id="quaryStudent" resultType="com.imooc.appoint.entiy.Student">
		SELECT 
			student_id,
			password
		FROM
			student
		WHERE
			student_id=#{studentId}
		AND password=#{password}		
	</select>

</mapper>
```
到此为止，我们dao层的开发就算结束了，大家可以写个测试类，测试一下。（我没写，哈哈），要实现预约功能还得有更多详细的逻辑组织，这个功能将他放在service中，他把DAO中与数据库交互的功能组织成可以用的详细逻辑。提供给上层web调用。<br>
## 8 service层<br>
### 首先写service接口类<br>
BookService.java
```java
package com.imooc.appoint.service;

import java.util.List;

import com.imooc.appoint.dto.AppointExecution;
import com.imooc.appoint.entiy.Appointment;
import com.imooc.appoint.entiy.Book;
import com.imooc.appoint.entiy.Student; 

public interface BookService {
	/**
	 * 查询一本图书
	 * 
	 * @param bookId
	 * @return
	 */
	Book getById(long bookId);  
	/**
	 * 查询所有图书
	 * 
	 * @return
	 */
	List<Book> getList();
	/**
	 * 登陆时查询数据库是否有该学生记录。
	 */
	Student validateStu(Long studentId,Long password);
	/**按照图书名称查询
	 * 按条件搜索图书
	 * 
	 */ 
	List<Book> getSomeList(String name);
	/*
	 * 查看某学生预约的所有图书
	 * 
	 */
	List<Appointment> getAppointByStu(long studentId);
	/**
	 * 预约图书
	 * 
	 * @param bookId
	 * @param studentId
	 * @return
	 */
	AppointExecution appoint(long bookId, long studentId);//返回预约成功的实体类
}
```
大家可以看到，这个借口类中基本和DAO中的没啥区别，有区别的是某些类他是在dao上更进一步，需要多个dao类一起组织，或者在加入其它逻辑才能实现<br>
为了实现BookService的借口，我们得写BookServiceImpl类。但是想让我们想想，为了写BookServiceImpl，我们需要什么，上面我们已经写出预约成功的实体类是AppointExecution，所以当然我们得写出该类，因为该类交互service和web，对这类有点像entiy包我们管叫bto包（bto包和其他包一起存放在appoint下）。对于AppointExecution来说，作用就是预约成功后给web层提供返回的信息。（就是返回预约成功、预约失败、无库存、之类的信息）<br>
```java
package com.imooc.appoint.dto;
import com.imooc.appoint.enums.AppointStateEnum;

public class AppointExecution {

	// 图书ID
	private long bookId;

	// 预约结果状态
	private int state;

	// 状态标识
	private String stateInfo;  

	public AppointExecution() {
	}

	// 预约失败的构造器
	public AppointExecution(long bookId, AppointStateEnum stateEnum) {
		this.bookId = bookId;
		this.state = stateEnum.getState();
		this.stateInfo = stateEnum.getStateInfo();
	}

	//set get 方法！
	public long getBookId() {
		return bookId;
	}

	public void setBookId(long bookId) {
		this.bookId = bookId;
	}

	public int getState() {
		return state;
	}

	public void setState(int state) {
		this.state = state;
	}

	public String getStateInfo() {
		return stateInfo;
	}

	public void setStateInfo(String stateInfo) {
		this.stateInfo = stateInfo;
	}
	
	@Override
	public String toString() {
		return "AppointExecution [bookId=" + bookId + ", state=" + state + ", stateInfo=" + stateInfo+"]";
	} 
}
```
除此之外，我们在预约图书时可能出现异常，例如重复预约、无库存、和其他异常，我们需要事先设计好异常类，来接收这类异常，方便处理，而不是直接报错。因此在appoint包下新建excption包，报下新建三个类：AppoinException.java;NoNumberException.java;RepeatAppoint.java<br>
AppoinException.java
```java
package com.imooc.appoint.exception;
//预约义务异常
public class AppointException extends RuntimeException{
	public AppointException(String message) {
		super(message);
	}

	public AppointException(String message, Throwable cause) {
		super(message, cause);
	}
}

```
NoNumberException.java
```java
package com.imooc.appoint.exception;

//库存不足异常
public class NoNumberException extends RuntimeException{
	public NoNumberException(String message) {
		super(message);
	}

	public NoNumberException(String message, Throwable cause) {
		super(message, cause);
	}

}

```
RepeatAppoint.java
```java
package com.imooc.appoint.exception;
//重复预约异常
public class RepeatAppointException extends RuntimeException{
	public RepeatAppointException(String message) {
		super(message);
	}

	public RepeatAppointException(String message, Throwable cause) {
		super(message, cause);
	}
}
```
现在可以写接口的实现类啦：BookServiceImpl.java<br>
```java
package com.imooc.appoint.service.Impl;

import java.util.List;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import com.imooc.appoint.dao.AppointmentDao;
import com.imooc.appoint.dao.BookDao;
import com.imooc.appoint.dao.StudentDao;
import com.imooc.appoint.dto.AppointExecution;
import com.imooc.appoint.entiy.Appointment;
import com.imooc.appoint.entiy.Book;
import com.imooc.appoint.entiy.Student;
import com.imooc.appoint.enums.AppointStateEnum;
import com.imooc.appoint.exception.AppointException;
import com.imooc.appoint.exception.NoNumberException;
import com.imooc.appoint.exception.RepeatAppointException;
import com.imooc.appoint.service.BookService;
 
@Service
public class BookServiceImpl implements BookService{
	private Logger logger=LoggerFactory.getLogger(this.getClass());
	@Autowired
	private BookDao bookDao;
	@Autowired
	private AppointmentDao appointmentDao;
	@Autowired
	private StudentDao studentDao; 
	@Override
	public Book getById(long bookId) { 
		return bookDao.queryById(bookId);
	}  
	@Override
	public List<Book> getList() { 
		return bookDao.queryAll(0, 1000);
	} 
	@Override
	public Student validateStu(Long studentId,Long password){
		return studentDao.quaryStudent(studentId, password);
	}
	@Override
	public List<Book> getSomeList(String name) {
		 
		return bookDao.querySome(name);
	} 
	@Override
	public List<Appointment> getAppointByStu(long studentId) {//DOTO
		return appointmentDao.quaryAndReturn(studentId);
		 
	}
	@Override
	@Transactional
	public AppointExecution appoint(long bookId, long studentId) {//在Dao的基础上组织逻辑，形成与web成交互用的方法
		try{													  //返回成功预约的类型。		
			int update=bookDao.reduceNumber(bookId);//减库存
			if(update<=0){//已经无库存！
				throw new NoNumberException("no number");
			}else{
				//执行预约操作
				int insert=appointmentDao.insertAppointment(bookId, studentId);
				if(insert<=0){//重复预约
					throw new RepeatAppointException("repeat appoint");
				}else{//预约成功
					return new AppointExecution(bookId,AppointStateEnum.SUCCESS);
				}
				
			}
		} catch (NoNumberException e1) {
			throw e1;
		} catch (RepeatAppointException e2) {
			throw e2;
		} catch (Exception e) {
			logger.error(e.getMessage(), e);
			// 所有编译期异常转换为运行期异常
			throw new AppointException("appoint inner error:" + e.getMessage());
		}
	} 
}

```
### 又到了我们写service层配置的时候！<br>
该xml依然位于resourse下的spring包下<br>
spring-service.xml
```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx.xsd">
	<!-- 扫描service包下所有使用注解的类型 -->
	<context:component-scan base-package="com.imooc.appoint.service"></context:component-scan>
	<!-- 配置事务管理器 -->
	<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<!-- 注入数据库连接池 -->
		<property name="dataSource" ref="dataSource" />
	</bean>
		<!-- 配置基于注解的声明式事务 -->
	<tx:annotation-driven transaction-manager="transactionManager" />
</beans>	
```

