---
layout: post
title: Spring boot - jackson 处理 JSON
date: 2020-03-05
Author: 山猪
tags: [Java, Spring Boot]
comments: true
---
![img](https://i2.wp.com/www.thecuriousdev.org/wp-content/uploads/2018/12/lombok-builder-jackson.jpg?w=800&ssl=1)

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
2. @JsonIgnoreProperties

    - @JsonIgnoreProperties是Jackson的注解，jackson1版本和2版本有区别，1版本只能放在类上，2版本类、方法、属性都可以，为了保持习惯，一般还是都放在model 类的类名上
    - ignoreUnknown属性，默认为false，此时反序列化json字符串时，有bean中没有的字段，就会抛出异常
    - value属性，默认为空，可以放入要忽略的字段，作用和@JsonIgnore一样，可以统一管理忽略字段


    ```java
    //生成json时将name和age属性过滤
    @JsonIgnoreProperties({"name"},{"age"})
    public class  user {
        private  String name;
        private int age;
    }
    ```
3. @JsonIgnoreType

    - 修饰类，忽略指定的类型的字段

4. @JsonProperty

    - @JsonProperty是Jackson的注解，jackson1版本和2版本区别很大，用于属性上、set/get方法上，该属性序列化后可重命名。
    - value属性，1、2版本一样，默认为""，代表该属性序列化和反序列化时的key值
    - required属性，2.0版本新增属性，默认false，2.6版本之后只能用于@JsonCreator中。例子中required=true，当反序列化时，json串中没有x或y，就会报错。不实用，一般不用该属性

    ```java
    @JsonProperty("nameJY")
    private String name;  // name值为 “暮色”
    ```

    ```java
    @JsonProperty("userIdStr")
    public String getUserIdStr() {
        return String.valueOf(getUserId());
    }
    ```

5. @JsonFormat

    - 此注解用于属性或者方法上（最好是属性上），可以方便的把Date类型直接转化为我们想要的模式，比如@JsonFormat(pattern = “yyyy-MM-dd HH-mm-ss”)

    ```java
    /**
     * 格式化日期属性
     */
    @JsonFormat(pattern = "yyyy-MM-dd")
    private Date birthday;
    ```

6. @JsonUnwrapped

    - 指定某个字段(类型是POJO)序列化成扁平化，而不是嵌套对象，在反序列化时再包装成对象

    ```java
    public class MyValue {
        public String name;
        @JsonUnwrapped(prefix = "pre_", suffix = "_suf")
        public MyValue myValue;
        public int age;
        public Date date;
    }

    //{"name":"杨正","pre_name_suf":null,"pre_age_suf":0,"pre_date_suf":null,"age":24,"date":"2017-12-09"}
    ```

7. @JsonView

    - 可以定义视图

    ```java
    class User {
        @JsonView({UserWithoutPassword.class})
        public String username;
        @JsonView({UserWithPassword.class})
        public String password;
        public interface UserWithPassword extends UserWithoutPassword {
        }
        public interface UserWithoutPassword {
        }
        @Override
        public String toString() {
            return "User{" +
                    "username='" + username + '\'' +
                    ", password='" + password + '\'' +
                    '}';
        }
    }

    public class JsonViewTest {
        public static void main(String[] args) throws IOException {
            ObjectMapper objectMapper = new ObjectMapper();
            String json = "{\"username\":\"dubby.cn\",\"password\":\"123456\"}";
            //反序列化，使用视图
            User user = objectMapper.readerWithView(User.UserWithoutPassword.class).forType(User.class).readValue(json);
            System.out.println(user);
            user.password = "xxxx";
            //序列化，使用视图
            String result1 = objectMapper.writerWithView(User.UserWithoutPassword.class).writeValueAsString(user);
            System.out.println(result1);
            String result2 = objectMapper.writerWithView(User.UserWithPassword.class).writeValueAsString(user);
            System.out.println(result2);
        }
    }
    ```

8. @JsonSerialize

    - 此注解用于属性或者getter方法上，用于在序列化时嵌入我们自定义的代码，比如序列化一个double时在其后面限制两位小数点。

9. @JsonDeserialize

    - 此注解用于属性或者setter方法上，用于在反序列化时可以嵌入我们自定义的代码，类似于上面的@JsonSerialize

