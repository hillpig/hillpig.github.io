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
    // 生成json时将name和age属性过滤
    @JsonIgnoreProperties(value={"name","age"})
    public class  user {
        private  String name;
        private int age;
        private String address;
    }

    // deserialization的时候会忽略除 name 和 age 之外的 条目
    @JsonIgnoreProperties(ignoreUnknown=true)
    public class  user {
        private  String name;
        private int age;
    }
    ```
3. @JsonIgnoreType

    - 修饰类
    - 忽略指定的类型的字段

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

    ```java
    /**
    * Note that as of 2.6, this property is only used for Creator
    * Properties, to ensure existence of property value in JSON:
    * for other properties (ones injected using a setter or mutable
    * field), no validation is performed. Support for those cases
    * may be added in future.
    * State of this property is exposed via introspection, and its
    * value is typically used by Schema generators, such as one for
    * JSON Schema.
    */
    public class MyClass {
        private Integer x;
        private Integer y;
        @JsonCreator
        public MyClass(@JsonProperty(value = "x", required = true) Integer x, @JsonProperty(value = "value_y", required = true) Integer y) {
            this.x = x;
            this.y = y;
        }
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
    public class ExampleMain {
        public static void main(String[] args) throws IOException {
        Department dept = new Department();
        dept.setName("Admin");
        dept.setLocation("NY");
        Employee employee = new Employee();
        employee.setName("Amy");
        employee.setDept(dept);

        System.out.println("-- before serialization --");
        System.out.println(employee);

        System.out.println("-- after serialization --");
        ObjectMapper om = new ObjectMapper();
        String jsonString = om.writeValueAsString(employee);
        System.out.println(jsonString);

        System.out.println("-- after deserialization --");
        Employee employee2 = om.readValue(jsonString, Employee.class);
        System.out.println(employee2);
        }
    }
    ```

    **输出**

    ```java
    -- before serialization --
    Employee{name='Amy', dept=Department{name='Admin', location='NY'}}
    -- after serialization --
    {"name":"Amy","dept-name":"Admin","dept-location":"NY"}
    -- after deserialization --
    Employee{name='Amy', dept=Department{name='Admin', location='NY'}}
    ```

    **如果没有prefix / suffix**

    ```java
    public class Employee {
        private String name;
        @JsonUnwrapped
        private Department dept;
        .............
    }
    ```

    **输出**

    ```java
    -- before serialization --
    Employee{name='Amy', dept=Department{name='Admin', location='NY'}}
    -- after serialization --
    {"name":"Amy","name":"Admin","location":"NY"}
    -- after deserialization --
    Employee{name='Admin', dept=Department{name='null', location='NY'}}
    ```

7. @JsonView

    - 可以定义视图

    ```java
    public class Views {
        public static class Public {
        }
    }

    public class User {
        public int id;
 
        @JsonView(Views.Public.class)
        public String name;
    }

    @Test
    public void whenUseJsonViewToSerialize_thenCorrect() 
      throws JsonProcessingException {
    
        User user = new User(1, "John");
    
        ObjectMapper mapper = new ObjectMapper();
        mapper.disable(MapperFeature.DEFAULT_VIEW_INCLUSION);
    
        String result = mapper
            .writerWithView(Views.Public.class)
            .writeValueAsString(user);
    
        assertThat(result, containsString("John"));
        assertThat(result, not(containsString("1")));
    }
    ```

8. @JsonSerialize 和 @JsonDeserialize

    - @JsonSerialize用于属性或者getter方法上，用于在序列化时嵌入我们自定义的代码，比如序列化一个double时在其后面限制两位小数点。
    - @JsonDeserialize用于属性或者setter方法上，用于在反序列化时可以嵌入我们自定义的代码，类似于上面的@JsonSerialize

    ```java
    public class CustomDoubleSerialize extends JsonSerializer<Double> {  
    
        private DecimalFormat df = new DecimalFormat("##.00");  
    
        @Override  
        public void serialize(Double value, JsonGenerator jgen,  
                SerializerProvider provider) throws IOException,  
                JsonProcessingException {  
    
            jgen.writeString(df.format(value));  
        }  
    }  
    ```

    ```java
    public class CustomDateDeserialize extends JsonDeserializer<Date> {  
  
        private SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");  
    
        @Override  
        public Date deserialize(JsonParser jp, DeserializationContext ctxt)  
                throws IOException, JsonProcessingException {  
    
            Date date = null;  
            try {  
                date = sdf.parse(jp.getText());  
            } catch (ParseException e) {  
                e.printStackTrace();  
            }  
            return date;  
        }  
    } 
    ```

    ```java
    //表示序列化时忽略的属性  
    @JsonIgnoreProperties(value = { "word" })  
    public class Person {  
        private String name;  
        private int age;  
        private boolean sex;  
        private Date birthday;  
        private String word;  
        private double salary;  
        
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
        
        public boolean isSex() {  
            return sex;  
        }  
        
        public void setSex(boolean sex) {  
            this.sex = sex;  
        }  
        
        public Date getBirthday() {  
            return birthday;  
        }  
        
        // 反序列化一个固定格式的Date  
        @JsonDeserialize(using = CustomDateDeserialize.class)  
        public void setBirthday(Date birthday) {  
            this.birthday = birthday;  
        }  
        
        public String getWord() {  
            return word;  
        }  
        
        public void setWord(String word) {  
            this.word = word;  
        }  
        
        // 序列化指定格式的double格式  
        @JsonSerialize(using = CustomDoubleSerialize.class)  
        public double getSalary() {  
            return salary;  
        }  
        
        public void setSalary(double salary) {  
            this.salary = salary;  
        }  
        
        public Person(String name, int age) {  
            this.name = name;  
            this.age = age;  
        }  
        
        public Person(String name, int age, boolean sex, Date birthday,  
                String word, double salary) {  
            super();  
            this.name = name;  
            this.age = age;  
            this.sex = sex;  
            this.birthday = birthday;  
            this.word = word;  
            this.salary = salary;  
        }  
        
        public Person() {  
        }  
        
        @Override  
        public String toString() {  
            return "Person [name=" + name + ", age=" + age + ", sex=" + sex  
                    + ", birthday=" + birthday + ", word=" + word + ", salary="  
                    + salary + "]";  
        }
    }  
    ```

[参考链接](https://www.concretepage.com/jackson-api/jackson-jsonignore-jsonignoreproperties-and-jsonignoretype#allowGetters)

[参考链接](https://www.baeldung.com/jackson-json-view-annotation)

[参考链接](https://www.jianshu.com/p/668aa8bda86f)