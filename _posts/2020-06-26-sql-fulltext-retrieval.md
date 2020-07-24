---
layout: post
title: SQL - Full Text Retrieval
date: 2020-06-20
Author: 山猪
tags: [Database, SQL]
comments: true
---
![img](https://intellipaat.com/mediaFiles/2015/11/SQL-e1559106221282.png)

<!-- more -->

## 版本支持

开始之前，先说一下全文索引的版本、存储引擎、数据类型的支持情况

1. MySQL 5.6 以前的版本，只有 MyISAM 存储引擎支持全文索引；
2. MySQL 5.6 及以后的版本，MyISAM 和 InnoDB 存储引擎均支持全文索引;
3. 只有字段的数据类型为 char、varchar、text 及其系列才可以建全文索引。

测试或使用全文索引时，要先看一下自己的 MySQL 版本、存储引擎和数据类型是否支持全文索引。


## 创建索引

1. 创建表时创建全文索引

    ```sql
    create table fulltext_test (
        id int(11) NOT NULL AUTO_INCREMENT,
        content text NOT NULL,
        tag varchar(255),
        PRIMARY KEY (id),
        FULLTEXT KEY content_tag_fulltext(content,tag)  // 创建联合全文索引列
    ) ENGINE=MyISAM DEFAULT CHARSET=utf8;
    ```

2. 在已存在的表上创建全文索引

    ```sql
    create fulltext index content_tag_fulltext
        on fulltext_test(content,tag);
    ```

3. 通过 SQL 语句 ALTER TABLE 创建全文索引

    ```sql
    alter table fulltext_test
        add fulltext index content_tag_fulltext(content,tag);
    ```

## 删除检索

1. 直接使用 DROP INDEX 删除全文索引

    ```sql
    drop index content_tag_fulltext
        on fulltext_test;
    ```

2. 通过 SQL 语句 ALTER TABLE 删除全文索引

    ```sql
    alter table fulltext_test
        drop index content_tag_fulltext;
    ```

## 使用全文检索

和常用的模糊匹配使用 like + % 不同，全文索引有自己的语法格式，使用 match 和 against 关键字，比如

```sql
select * from fulltext_test 
    where match(content,tag) against('xxx xxx');
```

注意： match() 函数中指定的列必须和全文索引中指定的列完全相同，否则就会报错，无法使用全文索引，这是因为全文索引不会记录关键字来自哪一列。如果想要对某一列使用全文索引，请单独为该列创建全文索引。

## 搜索长度

MySQL 中的全文索引，有两个变量，最小搜索长度和最大搜索长度，对于长度小于最小搜索长度和大于最大搜索长度的词语，都不会被索引。通俗点就是说，想对一个词语使用全文索引搜索，那么这个词语的长度必须在以上两个变量的区间内。

这两个的默认值可以使用以下命令查看：

```sql
show variables like '%ft%';
```

可以看到这两个变量在 MyISAM 和 InnoDB 两种存储引擎下的变量名和默认值

```console
// MyISAM
ft_min_word_len = 4;
ft_max_word_len = 84;

// InnoDB
innodb_ft_min_token_size = 3;
innodb_ft_max_token_size = 84;
```

可以看到最小搜索长度 MyISAM 引擎下默认是 4，InnoDB 引擎下是 3，也即，MySQL 的全文索引只会对长度大于等于 4 或者 3 的词语建立索引。

## 配置最小搜索长度

全文索引的相关参数都无法进行动态修改，必须通过修改 MySQL 的配置文件来完成。修改最小搜索长度的值为 1，首先打开 MySQL 的配置文件 /etc/my.cnf，在 [mysqld] 的下面追加以下内容

```console
[mysqld]
innodb_ft_min_token_size = 1
ft_min_word_len = 1
```
然后重启 MySQL 服务器，并修复全文索引。注意，修改完参数以后，一定要修复下索引，不然参数不会生效。

两种修复方式，可以使用下面的命令修复

```sql
repair table test quick;
```

或者直接删掉重新建立索引

## 两种全文索引

- 自然语言的全文索引

默认情况下，或者使用 in natural language mode 修饰符时，match() 函数对文本集合执行自然语言搜索，上面的例子都是自然语言的全文索引。

自然语言搜索引擎将计算每一个文档对象和查询的相关度。这里，相关度是基于匹配的关键词的个数，以及关键词在文档中出现的次数。在整个索引中出现次数越少的词语，匹配时的相关度就越高。相反，非常常见的单词将不会被搜索，如果一个词语的在超过 50% 的记录中都出现了，那么自然语言的搜索将不会搜索这类词语。上面提到的，测试表中必须有 4 条以上的记录，就是这个原因。

这个机制也比较好理解，比如说，一个数据表存储的是一篇篇的文章，文章中的常见词、语气词等等，出现的肯定比较多，搜索这些词语就没什么意义了，需要搜索的是那些文章中有特殊意义的词，这样才能把文章区分开。

- 布尔全文索引

在布尔搜索中，我们可以在查询中自定义某个被搜索的词语的相关性，当编写一个布尔搜索查询时，可以通过一些前缀修饰符来定制搜索。

MySQL 内置的修饰符，上面查询最小搜索长度时，搜索结果 ft_boolean_syntax 变量的值就是内置的修饰符，下面简单解释几个，更多修饰符的作用可以查手册

    + 必须包含该词
    - 必须不包含该词
    > 提高该词的相关性，查询的结果靠前
    < 降低该词的相关性，查询的结果靠后
    (*)星号 通配符，只能接在词后面

对于上面提到的问题，可以使用布尔全文索引查询来解决，使用下面的命令，a、aa、aaa、aaaa 就都被查询出来了。

```sql
select * test where match(content) against('a*' in boolean mode);
```

## 多语言支持

MySQL 的全文索引最开始仅支持英语，因为英语的词与词之间有空格，使用空格作为分词的分隔符是很方便的。亚洲文字，比如汉语、日语、汉语等，是没有空格的，这就造成了一定的限制。不过 MySQL 5.7.6 开始，引入了一个 ngram 全文分析器来解决这个问题，并且对 MyISAM 和 InnoDB 引擎都有效。

[参考原文1](https://zhuanlan.zhihu.com/p/35675553)

[参考原文2](https://www.zhihu.com/question/20596402)

[参考原文3](https://zhuanlan.zhihu.com/p/47581960)
