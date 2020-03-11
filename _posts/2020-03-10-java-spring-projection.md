---
layout: post
title: Spring boot - @Projection
date: 2020-03-10
Author: 山猪
tags: [Java, Spring Boot]
comments: true
---
![img](https://javadeveloperzone.com/wp-content/uploads/2018/09/Spring-JPA-Projection-example-1024x488.jpg)

<!-- more -->

## spring默认使用的是jackson处理json的序列化和反序列化

1. @JsonIgnore

    - @JsonIgnore是jackson的注解，通常当做标志注解(没有要赋值的属性)，常用于属性上。
    - JSON序列化反序列化都会忽略该属性。

    ```java
    public class user {
        private String name;
        @JsonIgnore
        private int age;
    }
    ```

