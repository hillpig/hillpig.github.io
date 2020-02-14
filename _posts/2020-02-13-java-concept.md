---
layout: post
title: 对一些java概念的理解
date: 2020-02-13
Author: 山猪
tags: [Java]
comments: true
---
![img](https://images.idgesg.net/images/article/2019/05/java_binary_code_gears_programming_coding_development_by_bluebay2014_gettyimages-1040871468_2400x1600-100795798-large.jpg)

<!-- more -->

## Composition

**Composition** is the **design technique** to implement *has-a* relationship in classes. We can use java inheritance or Object composition for code reuse. Java composition is achieved by using instance variables that refers to other objects.

Java composition is achieved by using instance variables that refers to other objects. Benefit of using composition is that we can control the visibility of other object to client classes and reuse only what we need. 

1. We can get runtime binding in composition where inheritance binds the classes at compile time. So composition provides flexibility in invocation of methods.

2. Composition allows us to provide restricted access to the methods and hence more secure.

3. Inheritance exposes all the super class methods and variables to client and if we have no control in designing superclass, 

4. Composition will never face this issue that any change in the superclass might affect subclass even though we might not be using the superclass methods

**Inheritance** is the concept of **designing** a class which defines properties and behavior which is common to set  of related classes.This class can be inherited by other classes the first is called super class and the second is called subclass.The subclass inherits all the instance variables and methods defined by the super class and can also add its own members. 

**Inheritance** represents the *IS-A* relationship, also known as parent-child relationship.

## How inheritance can be dangerous ?

Lets take an example

```java
public class X{    
   public void do(){    
   }    
}    
Public Class Y extends X{
   public void work(){    
       do();    
   }
}
```

As clear in above code , Class Y has very strong coupling with class X. If anything changes in superclass X , Y may break dramatically. Suppose In future class X implements a method work with below signature

```java
public int work(){
    // some code
}
```

Change is done in class X but it will make class Y uncompilable. SO this kind of dependency can go up to any level and it can be vary dangerous. Every time superclass might not have full visibility to code inside all its subclasses and subclass may be keep noticing what is happening in suerclass all the time. So we need to avoid this strong and unnecessary coupling.

How does composition solves this issue?

Lets see by revising the same example

```java
public class X{
    public void do(){
    }
}

Public Class Y{
    X x=new X();    
    public void work(){    
        x.do();
    }
}
```

Here we are creating reference of X class in Y class and invoking method of X class by creating an instance of X class. Now all that strong coupling is gone. Superclass and subclass are highly independent of each other now. Classes can freely make changes which were dangerous in inheritance situation.

## Second very good advantage of composition in that It provides method calling flexibility For example

```java
class X implements R{}
class Y implements R{}

public class Test{    
    R r;    
}
```

In Test class using r reference I can invoke methods of X class as well as Y class. This flexibility was never there in inheritance

## Another great advantage : Unit testing

```java
public class X{
    public void do(){
    }
}

Public Class Y{
    X x=new X();    
    public void work(){    
        x.do();    
    }    
}
```

In above example If state of x instance is not known ,it can easily be mocked up by using some test data and all methods can be easily tested. This was not possible at all in inheritance As you were heavily dependent on superclass to get the state of instance and execute any method.

## Another good reason why we should avoid inheritance is that Java does not support multiple inheritance.

```java
Public class Transaction {
    Banking b;
    public static void main(String a[])    
    {    
        b=new Deposit();    
        if(b.deposit()){    
            b=new Credit();
            b.credit();    
        }
    }
}
```

Good to know :

1. composition is easily achieved at runtime while inheritance provides its features at compile time

2. composition is also know as HAS-A relation and inheritance is also known as IS-A relation

So make it a habit of always preferring composition over inheritance for various above reasons.

## 区别和联系

### 区别

* 组合是一种 has-a 的 关系
* 继承是一种 is-a 的 关系

---

* 继承结构中，父类的内部细节对于子类是可见的。所以通过继承的代码复用是一种白盒式代码复用。
* 如果父类的实现跟随版本而发生改变，那么子类的实现也将随之改变。这样就导致了子类行为的不可预知性；

---

* 组合是通过对现有的类进行拼装（组合）产生新的、更复杂的功能。因为在类之间，各自的内部细节是不可见的，所以这种方式的代码复用是黑盒式代码复用。（因为组合中一般都定义一个类型，所以在编译期根本不知道具体会调用哪个实现类的方法）
* 继承，在写代码的时候就要指名具体继承哪个类，所以，在编译期就确定了关系。（从基类继承来的实现是无法在运行期动态改变的，因此降低了应用的灵活性。）
组合，在写代码的时候可以采用面向接口编程。所以，类的组合关系一般在运行期确定。

### 联系

* 组合和继承都是代码复用的一种方式

