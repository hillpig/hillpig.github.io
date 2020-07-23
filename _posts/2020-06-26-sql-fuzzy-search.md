---
layout: post
title: SQL - Fuzzy Search
date: 2020-06-20
Author: 山猪
tags: [Database, SQL]
comments: true
---
![img](https://350519-1085912-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/03/image-36.png)

<!-- more -->

# MySQL模糊搜索

## 四种模糊搜索

1. LIKE

    Like主要支持两种通配符，分别是"_"和"%"，其中前者代表匹配1个任意字符，常用于充当占位符；而后者代表匹配0个或多个任意字符。从某种意义上讲，Like可看作是一个精简的正则表达式功能。

    例如，在如上表中查找所有以"hello"开头的记录，则其SQL语句为：

    ```sql
    SELECT words FROM tests WHERE words LIKE 'hello%';
    ```

    如果想查找所有以"hello"开头且至少含有6个字符的记录，则可简单修改SQL语句如下：

    ```sql
    SELECT words FROM tests WHERE words LIKE 'hello_%';
    ```

    另外：当在Like模式字段中，若不包含任何"_"和"%"通配符，则等价于"="，表示精确匹配，例如查询语句……Like "hello"，则仅返回hello一条记录；还可在Like前加限定词Not，表示结果取反。

2. RegExp

    正则表达式，更加强大，MySQL语法支持绝大多数正则表达式功能。为了限定正则表达式以某个模式串开头或者结尾，可以通过添加"^"和"$"标识符来限定，例如仍然搜索以"hello"开头的目标字段，则其SQL语句为：

    ```sql
    SELECT words FROM tests WHERE words REGEXP 'hello';
    ```

3. 内置函数


    LOCATE(substr,str)  //返回substr在str中第一次出现的位置，LOCATE()只要找到返回的结果都大于0(即使是查询的内容就是最开始部分)，没有查找到才返回0
    LOCATE(substr,str,pos)  //同上，但是多了一个位置参数，返回sunstr在str中pos位置后第一次出现的位置
    INSTR(string ,substring)    //返回substring首次在string中出现的位置，据说是LOCATE()方法的别名函数，功能一样
    POSITION(substr IN str)  //同上，据说也是LOCATE()方法的别名函数，功能一样，不过它不再是通过返回值来判断，而是使用关键字IN
    FIND_IN_SET(str,strlist)    //strlist必须要是以逗号分隔的字符串，如果字符串str在strlist组成的N子串的字符串列表中存在，返回值的范围为1到N

    ```sql
    SELECT words  FROM tests WHERE INSTR(words, 'hello');  
    SELECT words  FROM tests WHERE LOCATE('hello', words);  
    SELECT words  FROM tests WHERE POSITION('hello' IN words); 
    ```

    注意：以上函数都不能使用索引

4. 全文索引

    全文索引是MySQL中索引的一种，曾经仅在引擎为MyISAM的表中支持，从5.6版本开始在InnoDB中也开始支持全文索引，支持的字段格式包括CHAR、VARCHAR和TEXT。在如上已经添加了全文索引的tests表中，仍然查询包含"hello"的记录，应用全文索引查询的SQL语句为：

    index 是前提条件，否则会报错： *SQL Error [1191] [HY000]: Can't find FULLTEXT index matching the column list*

    创建表，并建立索引

    ```sql
    CREATE TABLE IF NOT EXISTS tests(words TEXT, FULLTEXT (words));
    ```

    或者对已存在的表，创建相应的索引

    ```sql
    ALTER TABLE tests ADD FULLTEXT(words);
    ```

    之后可以进行搜索

    ```sql
    SELECT words FROM tests WHERE MATCH(words) against('hello');
    ```

    实际上，MATCH(words) against('hello')返回的是字段words对目标字符"hello"的匹配程度：当不存在任何匹配结果时，返回0；否则，根据匹配次数的多少和位置先后返回一个匹配度。例如，如下SQL语句返回表中每条记录对目标字段"hello"的匹配度

    ```sql
    SELECT MATCH(words) against('hello') FROM tests;
    ```

## 查询性能

基于索引的搜索一定是最快的，LIKE通配符次之但是和其他的方式差别不大。


[参考原文1](https://database.51cto.com/art/202004/614332.htm)



