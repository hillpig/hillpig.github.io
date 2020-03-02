---
layout: post
title: Spring boot中的 bean 和 IoC
date: 2020-01-27
Author: 山猪
tags: [Java, Spring Boot]
comments: true
---
![img](https://dzone.com/storage/temp/12766233-coffee-beans-photo.jpg?crop=1.00xw:0.676xh;0,0.199xh&resize=2048:*)

<!-- more -->

1. ## Overview

    Bean is a key concept of the Spring Framework. As such, understanding this notion is crucial to get the hang of the framework and to use it in an effective way.

    Unfortunately, **there aren't clear answers to a simple question – what a Spring bean really is.** Some explanations go to such a low level that a big picture is missed, whereas some are too vague.

2. ## Bean Definition
    Here's a definition of beans in [the Spring Framework documentation](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-introduction "the Spring Framework documentation"):

    *In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container.*

    This definition is concise and gets to the point, **but misses an important thing – Spring IoC container.** Let's go down the rabbit hole to see what it is and the benefits it brings in.

3. ## Inversion of Control

    Simply put, [Inversion of Control](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring "Inversion of Control"), or IoC for short, is a process in which an object defines its dependencies without creating them. This object delegates the job of constructing such dependencies to an IoC container.

    Let's start with the declaration of a couple of domain classes before diving into IoC.

    - ### Domain Classes

    Assume we have a class declaration:

    ```java
    public class Company {
        private Address address;
 
        public Company(Address address) {
            this.address = address;
    }
 
        // getter, setter and other properties
    }
    ```

    This class needs a collaborator of type Address:
    ```java
    public class Address {
        private String street;
        private int number;
    
        public Address(String street, int number) {
            this.street = street;
            this.number = number;
        }
    
        // getters and setters
    }
    ```
    - ### Tranditional Approach

    Normally, we create objects with their classes' constructors:

    ```java
    Address address = new Address("High Street", 1000);
    Company company = new Company(address);
    ```

    There's nothing wrong with this approach, but wouldn't it be nice to manage the dependencies in a better way?

    Imagine an application with dozens or even hundreds of classes. Sometimes we want to share a single instance of a class across the whole application, other times we need a separate object for each use case, and so on.

    Managing such a number of objects is nothing short of a nightmare. **This is where Inversion of Control comes to the rescue.**

    Instead of constructing dependencies by itself, an object can retrieve its dependencies from an IoC container. **All we need to do is to provide the container with appropriate configuration metadata.**

    - ### Bean configuration

    First off, let's decorate the Company class with the *@Component annotation:*
    ```java
    @Component
    public class Company {
        // this body is the same as before
    }
    ```
    Here's a configuration class supplying bean metadata to an IoC container:
    ```java
    @Configuration
    @ComponentScan(basePackageClasses = Company.class)
    public class Config {
        @Bean
        public Address getAddress() {
            return new Address("High Street", 1000);
        }
    }
    ```

    The configuration class produces a bean of type *Address*. It also carries the *@ComponentScan annotation*, which instructs the container to looks for beans in the package containing the *Company class*.

    **When a Spring IoC container constructs objects of those types, all the objects are called Spring beans as they are managed by the IoC container.**

    - ### IoC in Action

    Since we defined beans in a configuration class, **we'll need an instance of the AnnotationConfigApplicationContext class to build up a container:**

    ```java
    ApplicationContext context = new AnnotationConfigApplicationContext(Config.class);
    ```

    A quick test verifies the existence as well as property values of our beans:

    ```java
    Company company = context.getBean("company", Company.class);
    assertEquals("High Street", company.getAddress().getStreet());
    assertEquals(1000, company.getAddress().getNumber());
    ```

    The result proves that the IoC container has created and initialized beans correctly.

4. ## Conclusion

    This tutorial gave a brief description of Spring beans and their relationship with an IoC container.

    The complete source code for this tutorial can be found over on [GitHub](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring "GitHub").
