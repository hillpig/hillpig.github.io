---
layout: post
title: Spring boot - DTO vs DAO
date: 2020-02-05
Author: 山猪
tags: [Java, Spring Boot]
comments: true
---
![img](https://i.stack.imgur.com/3XnBN.png)

<!-- more -->

Data Access Object (DAO)
Data Transfer Object (DTO)

1. Data Access Object (DAO) 

    DAO is a class that usually has operations like save, update, delete. Whereas the DTO is just an object that holds data.

    The advantage of the DAO layer is that if you need to change the underlying persistence mechanism you only have to change the DAO layer, and not all the places in the domain logic where the DAO layer is used from.

    The advantage of DTO layer is that it adds a good deal of flexibility to the service layer and subsequently to the design of the entire application

2. Data Transfer Object (DTO)

    The DTO is used to expose several values in a bean like fashion. This provides a light-weight mechanism to transfer values over a network or between different application tiers.

    DTO will be passed as value object to DAO layer and DAO layer will use this object to persist data using its CRUD operation methods.



DTO is an abbreviation for Data Transfer Object, so it is used to transfer the data between classes and modules of your application.

    DTO should only contain private fields for your data, getters, setters, and constructors.
    DTO is not recommended to add business logic methods to such classes, but it is OK to add some util methods.

DAO is an abbreviation for Data Access Object, so it should encapsulate the logic for retrieving, saving and updating data in your data storage (a database, a file-system, whatever).

Here is an example of how the DAO and DTO interfaces would look like:

```java
interface PersonDTO {
    String getName();
    void setName(String name);
    //.....
}

interface PersonDAO {
    PersonDTO findById(long id);
    void save(PersonDTO person);
    //.....
}
```

The MVC is a wider pattern. The DTO/DAO would be your model in the MVC pattern.
It tells you how to organize the whole application, not just the part responsible for data retrieval.

As for the second question, if you have a small application it is completely OK, however, if you want to follow the MVC pattern it would be better to have a separate controller, which would contain the business logic for your frame in a separate class and dispatch messages to this controller from the event handlers.
This would separate your business logic from the view.



The problems with DAOs that I have seen is that they typically handle full objects all the time. This creates completely unneeded overhead that wouldn't exist with simple queries. For example, if a drop down is to be created off of database reference data, a DAO user may simply say: "Get me the collection of objects for this table where y ordered by z". Then, that data is used in the dropdown, but typically only for a key/value combination, ignoring everything else in the objects (created data, last user who updated it, whether or not it is active, etc) that was retrieved and mapped. Even if this massaging happens near the DAO call and the objects do not get stored as they are retrieved (which is typically not the case, unfortunately, the objects are often wrapped in a c:forEach (JSP) and iterated over to produce a drop down), it still creates unneeded database and network overhead, not to mention the temporary increase in memory to hold these objects.

Now, this is not to say that a DAO can't be designed to retrieve a Map of reference data - it certainly can. But typically they're used for the full object mapping, which is not what is needed all the time. It is a strength when saving, but a weakness, IMO, when retrieving data - sure, you get all of it - but often you don't need all of it, and it just wastes memory, bandwidth and time.

 DAO：
data   access   object数据访问对象
主要用来封装对数据库的访问。通过它可以把POJO持久化为PO，用PO组装出来VO、DTO 
 

 

DTO   ：
Data   Transfer   Object数据传输对象
主要用于远程调用等需要大量传输对象的地方。
比如我们一张表有100个字段，那么对应的PO就有100个属性。
但是我们界面上只要显示10个字段，
客户端用WEB   service来获取数据，没有必要把整个PO对象传递到客户端，
这时我们就可以用只有这10个属性的DTO来传递结果到客户端，这样也不会暴露服务端表结构.到达客户端以后，如果用这个对象来对应界面显示，那此时它的身份就转为VO

[ 参考 ](https://blog.csdn.net/zcywell/article/details/7204186 )