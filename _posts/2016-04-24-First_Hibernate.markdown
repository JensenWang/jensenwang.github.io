---
layout: post
title: Hibernate入门
date: 2016-04-24
tags: Tools
categor: Tools
---
## 简介
我们知道Java是面向对象的语言，但是现在大多数数据库是关系型数据库，以往我们要连接数据库通常使用的是JDBC（Java Database Connectivity，简称JDBC),但是JDBC在大型的项目中使用很复杂，有很大的编程成本，没有封装，难以实现MVC，个人认为最主要的还是sql语句的编写不是面向对象的。于是就出来了ORM，即Object Relational Mapping对象关系映射，是一种程序设计技术，用于实现面向对象编程语言里不同类型系统的数据之间的转换。其中Hibernate就是一种ORM的解决方案。

这里我会实现一个最最简单的hibernate小程序，仅供新手参考。首先搭建环境，我们要去Hibernate官网下载[Hibernate ORM][1]，数据库我使用的是[MySQL][2]，IDE是MyEclipse，还要[MySQL的驱动][3]

OK，开始干活
1. 在MyEclipse中新建一个Java项目，FirstHibernate
2. 导入jar包，打开我们下载的Hibernate压缩包，将hibernate-release-5.1.0.Final\lib\required\里面的jar包导入到MyEclipse中，右击项目，Build Path->Add External Archives...；同理，将下载的MySQL的驱动包导入
3. 创建模型在包com.hibernate.model下
{% highlight java %}
package com.hibernate.model;
public class Student {
    private int id;
    private String name;
    private int age;
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
{% endhighlight %}
4. 在src目录下创建Hibernate的配置文件，文件名为hibernate.cfg.xml，下载的hibernate包中有模板文件hibernate-release-5.1.0.Final\project\documentation\src\main\asciidoc\quickstart\tutorials\basic\src\test\resources\hibernate.cfg.xml;我配置的是：
{% highlight xml %}
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>

    <session-factory>

        <!-- Database connection settings -->
        <property name="connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="connection.url">jdbc:mysql://localhost/test</property>
        <property name="connection.username">root</property>
        <property name="connection.password">root</property>

        <!-- SQL dialect -->
        <property name="dialect">org.hibernate.dialect.MySQLDialect</property>

        <!-- Disable the second-level cache  -->
        <property name="cache.provider_class">org.hibernate.cache.internal.NoCacheProvider</property>

        <!-- Echo all executed SQL to stdout -->
        <property name="show_sql">true</property>

        <!-- Drop and re-create the database schema on startup -->
        <property name="hbm2ddl.auto">create</property>

        <mapping resource="com/hibernate/model/Student.hbm.xml"/>

    </session-factory>

</hibernate-configuration>
{% endhighlight %}
5. 在model包中(和Student目录在同一目录)创建一个Student.hbm.xml文件，模板在hibernate-release-5.1.0.Final\project\documentation\src\main\asciidoc\quickstart\tutorials\basic\src\test\java\org\hibernate\tutorial\hbm\Event.hbm.xml，当然要做一些改动。我的配置是：
{% highlight xml %}
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping package="com.hibernate.model">

    <class name="Student" table="Student">
        <id name="id" column="id">
            <generator class="increment"/>
        </id>
        <property name="name" type="string" column="name"/>
        <property name="age" type="int" column="age"/>
    </class>

</hibernate-mapping>
{% endhighlight %}
6. 写测试程序，创建一个com.hibernate.test包，在里面创建一个JUnit Test Case，名字叫HibernateTest.java
{% highlight java %}
package com.hibernate.test;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;
import org.junit.Test;

import com.hibernate.model.Student;

public class HibernateTest {
    @Test
    public void testStudent() {
        // 实例化对象，添加数据
        Student student = new Student();
        student.setAge(21);
        student.setName("JensenWang");
        // 加载配置文件
        Configuration config = new Configuration();
        config.configure();
        // 获取SessionFactory
        SessionFactory sf = config.buildSessionFactory();
        // 获取session对象
        Session session = sf.openSession();
        // 开启事务
        Transaction ts = session.beginTransaction();
        // 保存
        session.save(student);
        // 提交事务
        ts.commit();
        // 关闭
        session.close();
        sf.close();
    }
}
{% endhighlight %}
7. 创建数据库和表
{% highlight sql %}
create database test;
use test;
create table student(id int primary key, name varchar(20), age int);
{% endhighlight %}
8. 执行JUnit，如果没问题我们就可以看到表中写入了一条数据




  [1]: http://hibernate.org/orm/
  [2]: http://dev.mysql.com/downloads/
  [3]: http://dev.mysql.com/downloads/connector/j/
