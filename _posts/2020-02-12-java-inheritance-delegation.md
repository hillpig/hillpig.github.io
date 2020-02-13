---
layout: post
title: Delegation VS Inheritance - Java
date: 2020-02-12
Author: 山猪
tags: [Java]
comments: true
---
![img](https://dzone.com/storage/temp/12808609-classdiagramm.png)

<!-- more -->

[**Inheritance**](https://www.geeksforgeeks.org/inheritance-in-java/ "Inheritance") in Java programming is the process by which one class takes the property of another other class. i.e. the new classes, known as derived or child class, take over the attributes and behavior of the pre-existing classes, which are referred to as base classes or super or parent class.

**Delegation** is simply passing a duty off to someone/something else.

* Delegation can be an alternative to inheritance.
* Delegation means that you use an object of another class as an instance variable, and forward messages to the instance.
* It is better than inheritance for many cases because it makes you to think about each message you forward, because the instance is of a known class, rather than a new class, and because it doesn’t force you to accept all the methods of the super class: you can provide only the methods that really make sense.
* Delegation can be viewed as a relationship between objects where one object forwards certain method calls to another object, called its delegate.
* The primary advantage of delegation is run-time flexibility – the delegate can easily be changed at run-time. But unlike inheritance, delegation is not directly supported by most popular object-oriented languages, and it doesn’t facilitate dynamic polymorphism [dynamic polymorphism](https://www.geeksforgeeks.org/dynamic-method-dispatch-runtime-polymorphism-java/ "dynamic polymorphism").

```java
// Java program to illustrate 
// delegation 
class RealPrinter { 
    // the "delegate" 
    void print() 
    { 
        System.out.println("The Delegate"); 
    } 
} 
  
class Printer { 
    // the "delegator" 
    RealPrinter p = new RealPrinter(); 
  
    // create the delegate 
    void print() 
    { 
        p.print(); // delegation 
    } 
} 
  
public class Tester { 
  
    // To the outside world it looks like Printer actually prints. 
public static void main(String[] args) 
    { 
        Printer printer = new Printer(); 
        printer.print(); 
    } 
} 
```

Output:

> The Delegate

When you delegate, you are simply calling up some class which knows what must be done. You do not really care how it does it, all you care about is that the class you are calling knows what needs doing.

```java
// Java program to illustrate 
// Inheritance 
class RealPrinter { 
    // base class implements method 
    void print() 
    { 
        System.out.println("Printing Data"); 
    } 
} 3 // Printer Inheriting functionality of real printer 
    class Printer extends RealPrinter { 
  
    void print() 
    { 
        super.print(); // inside calling method of parent 
    } 
} 
  
public class Tester { 
  
    // To the outside world it looks like Printer actually prints. 
public static void main(String[] args) 
    { 
        Printer printer = new Printer(); 
        printer.print(); 
    } 
} 
```

Output:

> Printing Data

**When to use what?**

Here are some examples when inheritance or delegation are being used:

Assume your class is called B and the derived/delegated to class is called A then If

* You want to express relationship (is-a) then you want to use Inheritance.
* You want to be able to pass your class to an existing API expecting A’s then you need to use inheritance.
* You want to enhance A, but A is final and can no further be sub-classed then you need to use composition and delegation.
