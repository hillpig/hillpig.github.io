---
layout: post
title: Triple Dot in Java
date: 2020-03-15
Author: 山猪
tags: [Java]
comments: true
---
![img](http://www.mathcs.emory.edu/~cheung/Courses/170/Syllabus/09/FIGS/arg02.gif)

<!-- more -->

# 英文

The "Three Dots" in java is called the Variable Arguments or varargs. It allows the method to accept zero or multiple arguments. Varargs are very helpful if you don't know how many arguments you will have to pass in the method.

For Example:


> abc(); // Likely useless, but possible

> abc("x", "y", "z");

> abc("xyz");

> abc(new String[]{"x", "y", "z"});
    
But one thing you must not is, the argument that gets the ... must be the last in the method signature. In simpler terms, 

abc (int i, String... strings) is correct, whereas:

abc(String... strings, int i) is wrong.


[原文](https://www.edureka.co/community/31144/3-dot-in-parameter-in-java)

# 中文

类型后面三个点(String...)，是从Java 5开始，Java语言对方法参数支持一种新写法，叫可变长度参数列表，其语法就是类型后跟...，表示此处接受的参数为0到多个Object类型的对象，或者是一个Object[]。 例如我们有一个方法叫做test(String...strings)，那么你还可以写方法test()，但你不能写test(String[] strings)，这样会出编译错误，系统提示出现重复的方法。

在使用的时候，对于test(String...strings)，你可以直接用test()去调用，标示没有参数，也可以用去test("aaa")，也可以用test(new String[]{"aaa","bbb"})。

另外如果既有test(String...strings)函数，又有test()函数，我们在调用test()时，会优先使用test()函数。只有当没有test()函数式，我们调用test()，程序才会走test(String...strings)。
例一：

```java
public class Ttest {
    //private static int a;
    public static void test(int... a){
        for(int i=0;i<a.length;i++){
            System.out.println(a[i]);
        }
    }

    public static void main(String[] args) {
        Ttest.test(1,2);
    }
}
```
例二:
String... excludeProperty表示不定参数，也就是调用这个方法的时候这里可以传入多个String对象。

```java
public static void main(String[] args) {
    //测试，传入多个参数
    test("hello", "world", "13sd", "china", "cum", "ict");
}
 
public static void test(String... arguments) {
    for (int i = 0; i < arguments.length; i++) {
        System.out.println(arguments[i]);
    }
}

[原文](https://blog.csdn.net/guoquanyou/article/details/8571156)