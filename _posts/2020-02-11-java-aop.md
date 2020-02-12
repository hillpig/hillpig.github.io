---
layout: post
title: Spring AOP
date: 2020-02-10
Author: 山猪
tags: [Java, Spring boot]
comments: true
---
![img](https://www.javainuse.com/boot-66-18.JPG)

<!-- more -->

AOP is an abbreviation of Aspect-Oriented Programming.

* Cross-cutting concern

在DynamicProxyDemo專案的例子中，記錄的 動作原先被橫切（Cross-cutting）入至HelloSpeaker本身所負責的商務流程之中，另外類似於日誌這類的動作，如安全 （Security）檢查、交易（Transaction）等系統層面的服務（Service），在一些應用程式之中常被見到安插至各個物件的處理流程之 中，這些動作在AOP的術語中被稱之為Cross-cutting concerns。

以圖片說明可強調出Cross-cutting concerns的意涵，例如原來的商務流程是很單純的：

AOP(Aspect Oriented Programming) 面向切面编程，是目前软件开发中的一个热点，是Spring框架内容，利用AOP可以对业务逻辑的各个部分隔离，从而使的业务逻辑各部分的耦合性降低，提高程序的可重用性，踢开开发效率，主要功能：日志记录，性能统计，安全控制，事务处理，异常处理等。

AOP实现原理是java动态代理，但是jdk的动态代理必须实现接口，所以spring的aop是用cglib这个库实现的，cglis使用里asm这个直接操纵字节码的框架，所以可以做到不使用接口的情况下实现动态代理。

AOP与OOP的却别：

OOP面向对象编程，针对业务处理过程的实体及其属性和行为进行抽象封装，以获得更加清晰高效的逻辑单元划分。而AOP则是针对业务处理过程中的切面进行提取，它所面对的是处理过程的某个步骤或阶段，以获得逻辑过程的中各部分之间低耦合的隔离效果。这两种设计思想在目标上有着本质的差异。

举例：

对于“雇员”这样一个业务实体进行封装，自然是OOP的任务，我们可以建立一个“Employee”类，并将“雇员”相关的属性和行为封装其中。而用AOP 设计思想对“雇员”进行封装则无从谈起。

同样，对于“权限检查”这一动作片段进行划分，则是AOP的目标领域。

OOP面向名次领域，AOP面向动词领域。

