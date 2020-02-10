---
layout: post
title: Spring boot - DTO vs DAO
date: 2020-02-05
Author: 山猪
tags: [Java, Spring boot]
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

