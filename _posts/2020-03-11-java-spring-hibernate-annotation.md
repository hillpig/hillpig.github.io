---
layout: post
title: Spring boot - Hibernate 中的注释
date: 2020-03-11
Author: 山猪
tags: [Java, Spring Boot]
comments: true
---
![img](https://3.bp.blogspot.com/-sZpG0bMK5xQ/W_qZl9AuksI/AAAAAAAAE9U/2aimw3DF5pUZzeAOmN7xiLc59GTqRaNIQCEwYBhgL/s1600/spring-hibernate.jpg)

<!-- more -->

# 什么是Hibernate

    Hibernate是一种ORM框架，全称为 Object_Relative DateBase-Mapping，在Java对象与关系数据库之间建立某种映射，以实现直接存取Java对象


# 什么是ORM

    ORM是一种思想

    - O代表的是Objcet
    - R代表的是Relative
    - M代表的是Mapping

    ORM->对象关系映射....ORM关注是对象与数据库中的列的关系

    ![ORM](https://i.loli.net/2020/03/12/hirTEWDJ3Ybopn6.png)

# 什么是HQL语言？

    HQL：Hibernate Query Language

# HQL语言的语法是什么？

    HQL 的语法：就是将原来的 sql 语句中的表与字段名称换成对象与属性的名称就可以了

# getCurrentSession与openSession的区别

    getCurrentSession:
    
    当前session必须要有事务边界，且只能处理唯一的一个事务。当事务提交或者回滚后session自动失效

    openSeesion:
    
    每次都会打开一个新的session，假如每次使用多次。则获得是不同session对象。使用完毕后我们需要手动的调用colse()方法  关闭session

# 常用注释 General Annotation

1. @Entity

    *映射实体类*

    - name: 可选属性 ，指定对应表的名称，如果没有指定name属性，那么创建表的名称和类名一样


2. @Table

    *在实体类的上方使用，和Entity配合使用，指定实体类对应的数据库中的表的信息*

    - name：可选，指定表的名称，默认的是和类名一样，只有在不一致的情况下才会指定表名
    - catalog ： 可选，表示Catalog名称，默认为 Catalog(“”)
    - schema ： 可选 , 表示 Schema 名称 , 默认为Schema(“”)

3. @Id

    *表的主键*

4. @GeneratedValue(strategy=,generator="")

    *定义主键生成方式。 表示自增长*

    **Strategy的值：**

    - GenerationType.AUTO - 根据底层数据库自动选择（默认），若数据库支持自动增长类型，则为自动增长。
    - GenerationType.INDENTITY - 根据数据库的Identity字段生成，支持DB2、MySQL、 MS、SQL Server、SyBase与HyperanoicSQL数据库的Identity 类型主键。
    - GenerationType.SEQUENCE - 使用Sequence来决定主键的取值，适合Oracle、DB2等 支持Sequence的数据库，一般结合@SequenceGenerator使用。
    - GenerationType.TABLE - 使用指定表来决定主键取值，结合@TableGenerator使用。

    **generator:**
    
    - 表示主键生成器的名称,这个属性通常和ORM框架相关,例如,Hibernate可以指定uuid等主键生成方式.

5. @Column

    *定义了将成员属性映射到关系表中的哪一列和该列的结构信息*

    - name：可选，表示数据库表中该字段的名称，默认情形属性名称一致
    - nullable：可选，表示该字段是否允许为 null，默认为true
    - unique：可选，表示该字段是否是唯一标识，默认为 false
    - length：可选，表示该字段的大小，仅对 String 类型的字段有效，默认值255.
    - insertable：可选，表示在ORM框架执行插入操作时，该字段是否应出现INSETRT 语句中，默认为 true
    - updateable：可选，表示在ORM 框架执行更新操作时，该字段是否应该出现在 UPDATE语句中，默认为 true. 对于一经创建就不可以更改的字段，该 属性非常有用，如对于 birthday字段。
    - columnDefinition：可选，表示该字段在数据库中的实际类型。通常ORM框架可以根 据属性类型自动判断数据库中字段的类型，但是对于Date类型仍无法确定数据 库中字段类型究竟是 DATE,TIME还是 TIMESTAMP. 此外 ,String 的默认映射类型为VARCHAR, 如果要将 String 类型映射到特定数据库的 BLOB或 TEXT字段类型，该属性非常有用。

    ```java
    @Column(name="BIRTH",nullable="false",columnDefinition="DATE")
    private String bithday;
    ```

6. @Version

    *定义乐观锁*

7. @Basic

    *用于声明属性的存取策略：*

    - @Basic(fetch=FetchType.EAGER) 即时获取（默认的存取策略）
    - @Basic(fetch=FetchType.LAZY) 延迟获取

    - @Basic(optional=false) 表示该属性不允许为null,默认为true

8. @Temporal

    *设置数据库表中显示的日期的精度，因为java中的Date属性可以对应着数据库中的三种类型(DATE,TIME, TIMESTAMP)即是单纯的表示日期，时间，两者兼备的，默认的是两者兼备的，输出的是:2012-01-22 17:55:55*

    **三种的表示形式如下：**

    - TemporalType.TIME 输出到数据库中的仅仅是小时格式的，比如:12:22:12
    - TemporalType.DATE 输出到数据库中的是日期的格式：2012-12-01
    - TemporalType.TIMESTAMP 两者兼备，这个是默认的

9. @Transient

    *表示该属性并非一个到数据库表的字段的映射,ORM框架将忽略该属性.如果一个属性并非数据库表的字段映射,就务必将其标示为@Transient,否则,ORM框架默认其注解为@Basic*

10. @ManyToOne

    *表示一个多对一的映射,该注解标注的属性通常是数据库表的外键*
 
    - optional:是否允许该字段为null,该属性应该根据数据库表的外键约束来确定,默认为true
 
    - fetch:表示抓取策略,默认为FetchType.EAGER
 
    - cascade:表示默认的级联操作策略,可以指定为ALL,PERSIST,MERGE,REFRESH和REMOVE中的若干组合,默认为无级联操作
 
    - targetEntity:表示该属性关联的实体类型.该属性通常不必指定,ORM框架根据属性类型自动判断targetEntity.

    ```java
    //订单Order和用户User是一个ManyToOne的关系
    //在Order类中定义

    @ManyToOne()
    @JoinColumn(name="USER")
    public User getUser() {
        return user;
    }
    ```

11. @JoinColumn

    *@JoinColumn和@Column类似,介量描述的不是一个简单字段,而一一个关联字段*

    - name:该字段的名称.由于@JoinColumn描述的是一个关联字段,如ManyToOne,则默认的名称由其关联的实体决定.
 
    例如,实体Order有一个user属性来关联实体User,则Order的user属性为一个外键,
 
    其默认的名称为实体User的名称+下划线+实体User的主键名称, 见上例.

12. @OneToMany

    *描述一个一对多的关联,该属性应该为集体类型,在数据库中并没有实际字段.*

    - fetch:表示抓取策略,默认为FetchType.LAZY,因为关联的多个对象通常不必从数据库预先读取到内存
    - cascade:表示级联操作策略,对于OneToMany类型的关联非常重要,通常该实体更新或删除时,其关联的实体也应当被更新或删除
 
    *例如:实体User和Order是OneToMany的关系,则实体User被删除时,其关联的实体Order也应该被全部删除*

    ```java
    @OneTyMany(cascade=ALL)
    private List orders;
    ```

13. @OneToOne

    *描述一个一对一的关联*
 
    - fetch:表示抓取策略,默认为FetchType.LAZY
    - cascade:表示级联操作策略

14. @ManyToMany

    *描述一个多对多的关联.多对多关联上是两个一对多关联,但是在ManyToMany描述中,中间表是由ORM框架自动处理*
 
    - targetEntity:表示多对多关联的另一个实体类的全名,例如:package.Book.class
    - mappedBy:表示多对多关联的另一个实体类的对应集合属性名称
 
    *示例: User实体表示用户,Book实体表示书籍,为了描述用户收藏的书籍,可以在User和Book之间建立ManyToMany关联*

    ```java
    @Entity
    public class User {

        @ManyToMany(targetEntity=package.Book.class)
        private List books;
    }

    @Entity
    public class Book {

        @ManyToMany(targetEntity=package.Users.class,mappedBy="books")
        private List users;
    }
 
    两个实体间相互关联的属性必须标记为@ManyToMany,并相互指定targetEntity属性
 
    需要注意的是,有且只有一个实体的@ManyToMany注解需要指定mappedBy属性,指向targetEntity的集合属性名称
 
    利用ORM工具自动生成的表除了User和Book表外,还自动生成了一个User_Book表,用于实现多对多关联

## 例子 1：

```sql
CREATE TABLE EMPLOYEE (
    id INT NOT NULL auto_increment,
    first_name VARCHAR(20) NOT NULL,
    last_name  VARCHAR(20) NOT NULL,
    join_time   DATE default NULL,
    salary     INT  default NULL,
    PRIMARY KEY (id)
);
```

```java
import javax.persistence.*;

@Entity
@Table(name = "EMPLOYEE")
public class Employee {

    @Id 
    @GeneratedValue(strategy=GenerationType.AUTO) //设置主键自增长
    @Column(name = "id")
    private int id;

    @Column(name = "first_name", nullable = false) //设置名字不为空
    private String firstName;

    @Column(name = "last_name", nullable = false) //设置名字不为空
    private String lastName;

    @Temporal(TemporalType.DATE)   //设置时间精确到天数，2012-01-12
    @Column(name="join_time")   //改变表中字段的名字
    private Date joinDate;

    @Column(name = "salary")
    private int salary;

    @Transient   //设置该属性不在表中
    private String wife;  //妻子的名字

    public Employee() {}

    public int getId() {
        return id;
    }

    public void setId( int id ) {
        this.id = id;
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName( String first_name ) {
        this.firstName = first_name;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName( String last_name ) {
        this.lastName = last_name;
    }

    public Date getJoinDate() {
        return joinDate;
    }

    public void setJoinDate( Date joinDate ) {
        this.joinDate = joinDate;
    }

    public int getSalary() {
        return salary;
    }

    public void setSalary( int salary ) {
        this.salary = salary;
    }

    public String getWife() {
        return wife;
    }

    public void setWife(String wife) {
        this.wife = wife;
    }
}
```

## 例子 2：

```sql
CREATE TABLE vmtracka (
    id INT NOT NULL auto_increment,
    first_name VARCHAR(20) NOT NULL,
    last_name  VARCHAR(20) NOT NULL,
    join_time   DATE default NULL,
    salary     INT  default NULL,
    PRIMARY KEY (id)
);
```

```java
import java.io.Serializable;
import java.sql.Date;
import java.sql.Timestamp;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name="vmtracka")
public class Vmtracka implements Serializable{

    @Id
    @GeneratedValue(strategy=GenerationType.IDENTITY)
    private Long id;
    private Date gmt_create;
    
    private Timestamp gmt_update;//
    @Id
    @Column(nullable=false,length=20)
    private String device_id;
    
    @Column(columnDefinition="int(11) unsigned zerofill DEFAULT NULL Comment '层数'")
    private Integer vm_layer;//层数
    
    @Column(columnDefinition="int(11) DEFAULT NULL COMMENT '最大可放商品数'")
    private Integer vm_max_store;//最大可放商品数
    
    @Column(columnDefinition="int(11) unsigned zerofill DEFAULT NULL COMMENT '当前的库存数'")
    private Integer vm_now_store;//当前的库存数
    
    @Column(columnDefinition="varchar(100) DEFAULT '0' COMMENT '货号01'")
    private String goods_no_01;//货号01

    @Column(columnDefinition="varchar(100) DEFAULT '0' COMMENT '货号02'")
    private String goods_no_02;//货号02
    @Column(length=100)
    private String goods_no_03;
    @Column(length=100)
    private String goods_no_04;
    @Column(length=100)
    private String goods_no_05;
    @Column(length=100)
    private String goods_no_06;
    @Column(length=100)
    private String goods_no_07;
```

[Hibernate中注释的作用](https://zhuanlan.zhihu.com/p/91328953)

[Hibernate注解之基本注解的注解使用](https://zhuanlan.zhihu.com/p/35907884)

[Hibernate注解](https://www.yiibai.com/hibernate/hibernate_annotations.html)