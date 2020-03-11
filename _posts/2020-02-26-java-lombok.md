---
layout: post
title: Java - lombok
date: 2020-02-26
Author: 山猪
tags: [Java]
comments: true
---
![img](https://huongdanjava.com/wp-content/uploads/2017/08/lombok.jpg)

<!-- more -->

## Lombok是什么?

Lombok是一个通过注解以达到减少代码的Java库,如通过注解的方式减少get,set方法,构造方法等。

## 安装

IDEA => Settings => Plugins => Browse repositories => lombok ，点击安装，安装提示重启 IDEA，安装成功。

## 引入依赖

在自己的项目里添加 lombok 的编译支持，在 pom 文件里面添加 dependency

```java
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.16.18</version>
    <scope>provided</scope>
</dependency>
```

## 注释

1. @Data

    - 注解在 类 上
    - 提供类所有属性的 get 和 set 方法
    - 此外还提供了equals、canEqual、hashCode、toString 方法

    ```java
    // Java
    @Data
    public class Person {
        private String name;
        private String address;
        private Date brithday;
    }
    ```

    **等价：**

    ```java
    // Class
    public class Person {
        private String name;
        private String address;
        private Date birthday;

        public Message() {
        }

        public String getName() {return this.name;}

        public String getAddress() {return this.address;}

        public Date getBirthday() {return this.birthday;}

        public void setName(String name) {this.name = name;}

        public void setAddress(String address) {this.address = address;}

        public void setBirthday(Date birthday) {this.birthday = birthday;}

        public boolean equals(Object o) {...}

        public boolean canEqual(Object o) { return other instanceof Message;}

        public int hashCode() {...}

        public String toString() {
            return "Message(name=" + this.getName() + ", address=" + this.getAddress() + ", birthday=" + this.getBirthday() );
        }
    }
    ```


2. @Setter

    - 注解在 属性 上
    - 为单个属性提供 set 方法
    - 注解在 类 上，为该类所有的属性提供 set 方法， 都提供默认构造方法。

    **注释在class上**

    ---

    ```java
    // Java
    @Setter
    public class Person {
        private String name;
        private String address;
        private Date brithday;
    }
    ```

    **等价：**

    ```java
    // Class
    public class Person {
        private String name;
        private String address;
        private Date birthday;

        public Message() {
        }

        public void setName(String name) {this.name = name;}

        public void setAddress(String address) {this.address = address;}

        public void setBirthday(Date birthday) {this.birthday = birthday;}
    }
    ```

    **注释在variable上**

    ---

    ```java
    // Java
    public class Person {

        @Setter
        private String name;
        private String address;
        private Date brithday;
    }
    ```

    **等价：**

    ```java
    // Class
    public class Person {
        private String name;
        private String address;
        private Date birthday;

        public Message() {
        }

        public void setName(String name) {this.name = name;}
    }
    ```

3. @Getter

    - 注解在 属性 上
    - 为单个属性提供 get 方法
    - 注解在 类 上，为该类所有的属性提供 get 方法，都提供默认构造方法。


    **注释在class上**

    ---

    ```java
    // Java
    @Getter
    public class Person {
        private String name;
        private String address;
        private Date brithday;
    }
    ```

    **等价：**

    ```java
    // Class
    public class Person {
        private String name;
        private String address;
        private Date birthday;

        public Message() {
        }

        public String getName() {return this.name;}

        public String getAddress() {return this.address;}

        public Date getBirthday() {return this.birthday;}
    }
    ```

    **注释在variable上**

    ---

    ```java
    // Java
    public class Person {
        private String name;
        private String address;

        @Getter
        private Date brithday;
    }
    ```

    **等价：**

    ```java
    // Class
    public class Person {
        private String name;
        private String address;
        private Date birthday;

        public Message() {
        }

        public Date getBirthday() {return this.birthday;}
    }
    ```

4. @Log4j

    - 注解在 类 上
    - 为类提供一个 属性名为 log 的 log4j 日志对象，提供默认构造方法

    ```java
    // Java
    @Slf4j
    public class StudentTest {
        public static void main(String[] args) {
            log.info("Here is some INFO");
        }
    }
    ```

    **等价：**

    ```java
    // Class
    public class StudentTest {
        public static Logger log = LoggerFactory.getLogger(StudentTest.class);
        public static void main(String[] args) {
            log.info("Here is some INFO");
        }
    }
    ```


5. @AllArgsConstructor

    - 注解在 类 上
    - 为类提供一个全参的构造方法，加了这个注解后，类中不提供默认构造方法了。

    ```java
    @AllArgsConstructor
    public class Student {
        private long id = new Long(0);
        private String name = " ";
        private String className = " ";
    }
    ```

    **等价：**

    ```java
    public class Student {
        private long id = new Long(0);
        private String name = " ";
        private String className = " ";

        public Student(long id, String name, String className) {
            this.id = id;
            this.name = name;
            this.className = className;
        }
    }
    ```

6. @NoArgsConstructor

    - 注解在 类 上
    - 为类提供一个无参的构造方法。

    **例 1**

    ---

    ```java
    @NoArgsConstructor
    public class Student {
        private long id = new Long(0);
        private String name = " ";
        private String className = " ";
    }
    ```
    
    **等价：**

    ```java
    public class Student {
        private long id = new Long(0);
        private String name = " ";
        private String className = " ";

        public Student() {
        }
    }
    ```

    **例 2**

    ---

    ```java
    @NoArgsConstructor(staticName = "init")
    public class Student {
        private long id = new Long(0);
        private String name = " ";
        private String className = " ";
    }
    ```
    
    **等价：**

    ```java
    public class Student {
        private long id = new Long(0);
        private String name = " ";
        private String className = " ";

        public static Student init(){
            return new Student();
        }
    }
    ```


    **调用：**

    ```java
    public class StudentTest {
        public static void main(String[] args) {
        //    Student student = new Student();//编译时报错
            Student student = Student.init();
            student.setName("hresh");
            student.setClassName("Lv5");
            System.out.println(student);
        }
    }
    ```

7. @EqualsAndHashCode

    - 注解在 类 上,
    - 可以生成 equals、canEqual、hashCode 方法。

    ```java
    //父类
    public class Person {
        private String name;
        private String sex;

        public Person(String name, String sex) {
            this.name = name;
            this.sex = sex;
        }
    }

    // 子类
    @EqualsAndHashCode(exclude = {"className"},callSuper = false)
    public class Student extends Person{
    
        @Getter
        @Setter
        private int age;

        @Getter
        @Setter
        private String className;
    
        public Student(String name, String sex, int age, String className) {
            super(name,sex);
            this.age = age;
            this.className = className;
        }

    }
    ```

    **调用：**

    ---

    ```java
    Student s1 = new Student("hresh","man",22,"Lv3");
    Student s2 = new Student("hresh","woman",22,"Lv5");
    System.out.println(s1.equals(s2));//true
    }
    ```

    **解析：** 
    
    子类实现@EqualsAndHashCode(callSuper = false) ，不调用父类的属性，那么子类属性里面的相同的话，那 hashcode 的值就相同，再加上排除对 className 属性的比对，所以代码里面的2个对象的 equals 方法的返回值是 true 。

8. @NonNull

    - 注解在 属性 上
    - 自动产生一个关于此参数的非空检查，如果参数为空，则抛出一个空指针异常，也会有一个默认的无参构造方法。

    ```java
    public class Person {

        private String name;

        @Setter
        @Getter
        @NonNull
        private List<Person> member;
    }
    ```

    **等价：**

    ```java
    @NonNull
    private List<Person> members;
    
    public Family(@NonNull final List<Person> members) {
        if (members == null) throw new java.lang.NullPointerException("members");
        this.members = members;
    }
    
    @NonNull
    public List<Person> getMembers() {
        return members;
    }
    
    public void setMembers(@NonNull final List<Person> members) {
        if (members == null) throw new java.lang.NullPointerException("members");
        this.members = members;
    }
    ```

9. @Cleanup

    - 注解用在 变量 上
    - 可以保证此变量代表的资源会被自动关闭，默认是调用资源的 close() 方法，如果该资源有其它关闭方法，可使用 @Cleanup(“methodName”) 来指定要调用的方法，
    - 也会生成默认的构造方法

    ```java
    public void testCleanUp() {
        try {
            @Cleanup
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            baos.write(new byte[] {'Y','e','s'});
            System.out.println(baos.toString());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    ```

    **等价：**

    ```java
    public void testCleanUp() {
        try {
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            try {
                baos.write(new byte[]{'Y', 'e', 's'});
                System.out.println(baos.toString());
            } finally {
                baos.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    ```

10. @ToString

    - 注解在 类 上
    - 可以生成所有参数的 toString 方法
    - 还会生成默认的构造方法

    *callSuper* 是否输出父类的toString方法,默认为false
    *includeFieldNames* 是否包含字段名称,默认为true
    *exclude* 排除生成tostring的字段

    ```java
    @ToString(callSuper = true,exclude ={"name"})
    public class Person {
        private String name;
        private String address;
    }
    ```

    **等价：**

    ```java
    public class Person {
        private String name;
        private String address;

        public Person() {
        }

        public String toString() {
            return "Person{name=" + this.name + ", address=" + this.address + "}";
        }
    }
    ```

11. @RequiredArgsConstructor

    - 注解用在 类 上，
    - 使用类中所有带有 @NonNull 注解的或者带有 final 修饰的成员变量生成对应的构造方法。

    ```java
    @RequiredArgsConstructor
    public class Student {
        @NonNull
        private long id;
        @NonNull
        private String name;
        private String className;
    }
    ```

    **等价：**

    ```java
    public class Student {
        @NonNull
        private long id;
        @NonNull
        private String name;
        private String className;

        @ConstructorProperties({"id","name"})
        public Student(@NonNull long id, @NonNull String name) {
            if (id == null) {
                throw new NullPointerException("id");
            } if else (name == null) {
                throw new NullPointerException("name");
            } else {
                this.id = id;
                this.name = name;
            }
        }
    }
    ```

12. @Value

    - 注解用在 类 上
    - 会生成含所有参数的构造方法，get 方法，没有 set 方法
    - 此外还提供了equals、hashCode、toString 方法。

    ```java
    @Value
    @AllArgsConstructor
    public class Student {
        private String name;
        private int age;
    }
    ```

    **等价：**

    ```java
    @Value
    public class Student {
        private String name;
        private int age;

        @ConstructorProperties({"name","age"})
        public Student(String name, int age) {
            this.name = name;
            this.age = age;
        }

        private final int age;
        public int getName() {
            return this.name;
        }

        public int getAge() {
            return this.age;
        }

        // 省略 equals、 hashcode、 toString 方法
    }
    ```

    **调用：**

    ```java
    Student student = new Student("hresh",22);//没有set方法
    System.out.println(student);
    System.out.println(student.getName());
    ```

13. @SneakyThrows

    - 注解在 方法 上
    - 可以将方法中的代码用 try-catch 语句包裹起来，捕获异常并在 catch 中用 Lombok.sneakyThrow(e) 把异常抛出
    - 可以使用 @SneakyThrows(Exception.class) 的形式指定抛出哪种异常
    - 会生成默认的构造方法。

    ```java
    public class Test {

        @SneakyThrows
        public static void main(String[] args){
            InputStream inputStream = new FileInputStream("");
        }
    }
    ```

    **等价：**

    ```java
    public class Test {

        public Test(){
        }

        public static void main(String[] args){
            try {
                new FileInputStream("");
            } catch (IOException var2) {
                throw var2;
            }
        }
    }
    ```

14. @Synchronized

    - 注解在 类方法 或者 实例方法 上
    - 效果和 synchronized 关键字相同，区别在于锁对象不同，对于类方法和实例方法，synchronized 关键字的锁对象分别是类的 class 对象和 this 对象，而 @Synchronized 的锁对象分别是 私有静态 final 对象 lock 和 私有 final 对象 lock，当然，也可以自己指定锁对象
    - 此外也提供默认的构造方法

    **例子 1**

    ---

    ```java
    public class Test {

        private final Object readLock = new Object();

        @Synchronized
        public static void hello() {
            System.out.println("hello");
        }

        @Synchronized
        public int answer() {
            return 518;
        }

        @Synchronized("readLock")
        public void foo() {
            System.out.println("foo")
        }
    }
    ```

    **等价：**

    ```java
    public class Test {
        private static final Object $LOCK = new Object[0];
        private final Object $lock = new Object[0];
        private final Object readLock = new Object();

        public Test() {}

        public static void hello() {
            Object var0 = $LOCK;
            synchronized($LOCK) {
                System.out.println("hello");
            }
        }

        public int answer() {
            Object var1 = this.$lOCK;
            synchronized(this.$lOCK) {
                return 518;
            }
        }

        public static void hello() {
            Object var2 = this.readLock;
            synchronized(this.readLock) {
                System.out.println("foo");
            }
        }
    }
    ```

    **例子 2**

    ---

    ```java
    private DateFormat format = new SimpleDateFormat("MM-dd-YYYY");
    
    @Synchronized
    public String synchronizedFormat(Date date) {
        return format.format(date);
    }
    ```

    **等价：**

    ```java
    private final java.lang.Object $lock = new java.lang.Object[0];
    private DateFormat format = new SimpleDateFormat("MM-dd-YYYY");
    
    public String synchronizedFormat(Date date) {
        synchronized ($lock) {
            return format.format(date);
        }
    }
    ```

15. @Builder

    - 用在类、构造器、方法上
    - 提供复杂的 builder APIs

    ```java
    @Builder
    @Data
    public class Student {
        private String name;
        private int age;
    }
    ```

    **等价：**

    ```java
    public class Student {
        private String name;
        private int age;
        public String getName() {
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }
        public int getAge() {
            return age;
        }
        public void setAge(int age) {
            this.age = age;
        }
        public static Builder builder(){
            return new Builder();
        }
        public static class Builder{
            private String name;
            private int age;
            public Builder name(String name){
                this.name = name;
                return this;
            }
            public Builder age(int age){
                this.age = age;
                return this;
            }
            public Student build(){
                Student student = new Student();
                student.setAge(age);
                student.setName(name);
                return student;
            }
        }
    }
    ```

    **调用：**

    ```java
    public class StudentTest {
        public static void main(String[] args) {
            Student student = Student.builder().name("hresh").age(22).build();
        }
    }
    ```

[参考链接](https://juejin.im/post/5dde71ddf265da06051d00ab "掘金")

[参考链接](https://zhuanlan.zhihu.com/p/32779910 "Lombok 看这篇就够了")





