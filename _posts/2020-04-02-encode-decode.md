---
layout: post
title: HTTP - Header encode & decode
date: 2020-04-02
Author: 山猪
tags: [http]
comments: true
---
![img](https://miro.medium.com/max/1012/1*b_fkruR5t9-r_t5Tj1JocQ.png)

<!-- more -->

## Encode & Decode 逻辑

Python在做编码转换时，通常需要以unicode作为中间编码

即先将其他编码的字符串解码（decode）成unicode，再从unicode编码（encode）成另一种编码。

decode的作用是将其他编码的字符串转换成unicode编码，如str1.decode('gb2312')，表示将gb2312编码的字符串str1转换成unicode编码。

encode的作用是将unicode编码转换成其他编码的字符串，如str2.encode('gb2312')，表示将unicode编码的字符串str2转换成gb2312编码。

总得意思:想要将其他的编码转换成utf-8必须先将其解码成unicode然后重新编码成utf-8,它是以unicode为转换媒介的

如：

s='中文'

如果是在utf8的文件中，该字符串就是utf8编码，如果是在gb2312的文件中，则其编码为gb2312。这种情况下，要进行编码转换，都需要先用decode方法将其转换成unicode编码，再使用encode方法将其转换成其他编码。通常，在没有指定特定的编码方式时，都是使用的系统默认编码创建的代码文件。

如：

s.decode('utf-8').encode('utf-8')

decode()是解码
encode()是编码
isinstance(s,unicode)

判断s是否是unicode编码，如果是就返回true,否则返回false*

## FORM元素的enctype属性指定了表单数据向服务器提交时所采用的编码类型

### application/x-www-form-urlencoded

这是通过表单发送数据时默认的编码类型。我们没有在from标签中设置enctype属性时默认就是application/x-www-form-urlencoded类型的。application/x-www-form-urlencoded编码类型会把表单中发送的数据编码为“名称/值”对。这是标准的编码格式。当表单的ACTION为POST的时候，浏览器把form数据封装到http body中，然后发送到服务器。当表单的ACTION为GET的时候，application/x-www-form-urlencoded编码类型会把表单中发送的数据转换成一个字符串(name=coderbolg&key=php),然后把这个字符串附加到URL后面，并用?分割，接着就请求这个新的URL。当我们通过POST方式向服务器发送AJAX请求时最好要通过设置请求头来指定为application/x-www-form-urlencoded编码类型。方法是在xmlobject.open()方法之后添加xmlobject.setRequestHeader("Content-Type","application/x-www-form-urlencoded") 不然服务器会接收不到POST过来的数据。

### multipart/form-data

然而，在向服务器发送大量的文本、包含非ASCII字符的文本或二进制数据时“application/x-www-form-urlencoded”这种编码方式效率很低。在文件上载时，所使用的编码类型应当是“multipart/form-data”，它既可以发送文本数据，也支持二进制数据上载。

Browser端<form>表单的ENCTYPE属性值为multipart/form-data，它告诉我们传输的数据要用到多媒体传输协议，由于多媒体传输的都是大量的数据，所以规定上传文件必须是post方法，<input>的type属性必须是file。(表单里有图片上传用ENCTYPE="multipart/form-data")。

```xml
<form name="userInfo" method="post" action="first_submit.jsp"    ENCTYPE="multipart/form-data">
```

表单标签中设置enctype="multipart/form-data"来确保匿名上载文件的正确编码。如下：

```xml
<tr>
      <td height="30" align="right">上传企业营业执照图片：</td>
      <td><INPUT TYPE="FILE" NAME="uploadfile" SIZE="34"    onChange="checkimage()"></td>
</tr>
```

当表单中有file类型控件并希望它正常工作的话，就必须设置成multipart/form-data类型，浏览器会把整个表单以控件为单位分割，并为每个部分加上Content-Disposition(form-data或者file),Content-Type(默认为text/plain),name(控件 name)等信息，并加上分割符（boundary)。

 

注：enctype="multipart/form-data"是上传二进制数据; form里面的input的值以2进制的方式传过去。

form里面的input的值以2进制的方式传过去，所以request就得不到值了。 也就是说加了这段代码,用request就会传递不成功,取表单值加入数据库时，用到下面的：

SmartUpload su = new SmartUpload();//新建一个SmartUpload对象

su.getRequest().getParameterValues();取数组值

su.getRequest().getParameter( );取单个参数单个值

### text/plain

数据以纯文本形式进行编码，其中不含任何控件或格式字符

[参考原文1](https://segmentfault.com/a/1190000015788943)

[参考原文2](https://towardsdatascience.com/a-guide-to-unicode-utf-8-and-strings-in-python-757a232db95c)

[参考原文3](https://www.iteye.com/blog/mikzhang-1101705)



