---
layout: post
title: Spring boot 事务特性　@Transactional
date: 2020-02-10
Author: 山猪
tags: [Java, Spring Boot]
comments: true
---
![img](https://www.javainuse.com/boot-66-18.JPG)

<!-- more -->

* 常见坑点1：遇到检测异常时，事务默认不回滚。

    例如下面这段代码，账户余额依旧增加成功，并没有因为后面遇到SQLException（检测异常）而进行事务回滚！！

    ```java
        @Transactional
        public void addMoney() throws Exception {
            //先增加余额
            accountMapper.addMoney();
            //然后遇到故障
            throw new SQLException("发生异常了..");
        }
    ```
    原因分析：因为Spring的默认的事务规则是遇到运行异常（RuntimeException及其子类）和程序错误（Error）才会进行事务回滚，显然SQLException并不属于这个范围。如果想针对检测异常进行事务回滚，可以在@Transactional 注解里使用rollbackFor 属性明确指定异常。例如下面这样，就可以正常回滚：

    ```java
        @Transactional(rollbackFor = Exception.class)
        public void addMoney() throws Exception {
            //先增加余额
            accountMapper.addMoney();
            //然后遇到故障
            throw new SQLException("发生异常了..");
        }
    ```

* 常见坑点2： 在业务层捕捉异常后，发现事务不生效。

    这是许多新手都会犯的一个错误，在业务层手工捕捉并处理了异常，你都把异常“吃”掉了，Spring自然不知道这里有错，更不会主动去回滚数据。例如：下面这段代码直接导致增加余额的事务回滚没有生效。

    ```java
    @Transactional
    public void addMoney() throws Exception {
        //先增加余额
        accountMapper.addMoney();
        //谨慎：尽量不要在业务层捕捉异常并处理
        try {
            throw new SQLException("发生异常了..");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    ```

    不要小瞧了这些细节，往前暴露异常很大程度上很能够帮我们快速定位问题，而不是经常在项目上线后出现问题，却无法刨根知道哪里报错。

    推荐做法：若非实际业务要求，则在业务层统一抛出异常，然后在控制层统一处理。

    ```java
    @Transactional
    public void addMoney() throws Exception {
        //先增加余额
        accountMapper.addMoney();
        //推荐：在业务层将异常抛出
        throw new RuntimeException("发生异常了..");
    }
    ```
