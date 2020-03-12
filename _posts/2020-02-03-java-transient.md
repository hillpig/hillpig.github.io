---
layout: post
title: transient 的作用
date: 2020-02-05
Author: 山猪
tags: [Java]
comments: true
---
![img](https://4.bp.blogspot.com/-uLmc1hOq1fE/WP-acf2E4rI/AAAAAAAAoM8/UJvPps1Vx84kMWoCl4POGFovxJ6cx-oVQCLcB/s400/transient.JPG)

<!-- more -->

1. 一旦变量被transient修饰，变量将不再是对象持久化的一部分，该变量内容在序列化后无法获得访问。

    ```java
    import java.io.FileInputStream;
    import java.io.FileNotFoundException;
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.io.ObjectInputStream;
    import java.io.ObjectOutputStream;
    import java.io.Serializable;

    /**
    * @description 使用transient关键字不序列化某个变量
    *        注意读取的时候，读取数据的顺序一定要和存放数据的顺序保持一致
    *        
    * @author Alexia
    * @date  2013-10-15
    */
    public class TransientTest {
        
        public static void main(String[] args) {
            
            User user = new User();
            user.setUsername("Alexia");
            user.setPasswd("123456");
            
            System.out.println("read before Serializable: ");
            System.out.println("username: " + user.getUsername());
            System.err.println("password: " + user.getPasswd());
            
            try {
                ObjectOutputStream os = new ObjectOutputStream(
                        new FileOutputStream("C:/user.txt"));
                os.writeObject(user); // 将User对象写进文件
                os.flush();
                os.close();
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                ObjectInputStream is = new ObjectInputStream(new FileInputStream(
                        "C:/user.txt"));
                user = (User) is.readObject(); // 从流中读取User的数据
                is.close();
                
                System.out.println("\nread after Serializable: ");
                System.out.println("username: " + user.getUsername());
                System.err.println("password: " + user.getPasswd());
                
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }
        }
    }

    class User implements Serializable {
        private static final long serialVersionUID = 8294180014912103005L;  
        
        private String username;
        private transient String passwd;
        
        public String getUsername() {
            return username;
        }
        
        public void setUsername(String username) {
            this.username = username;
        }
        
        public String getPasswd() {
            return passwd;
        }
        
        public void setPasswd(String passwd) {
            this.passwd = passwd;
        }

    }
    ```

    *Output*

    ```java
    read before Serializable: 
    username: Alexia
    password: 123456

    read after Serializable: 
    username: Alexia
    password: null
    ```

    **密码字段为null，说明反序列化时根本没有从文件中获取到信息**

2. transient关键字只能修饰变量，而不能修饰方法和类。注意，本地变量是不能被transient关键字修饰的。变量如果是用户自定义类变量，则该类需要实现Serializable接口。

3. 被transient关键字修饰的变量不再能被序列化，一个静态变量不管是否被transient修饰，均不能被序列化。

    ```java
    import java.io.FileInputStream;
    import java.io.FileNotFoundException;
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.io.ObjectInputStream;
    import java.io.ObjectOutputStream;
    import java.io.Serializable;

    /**
    * @description 使用transient关键字不序列化某个变量
    *        注意读取的时候，读取数据的顺序一定要和存放数据的顺序保持一致
    *        
    * @author Alexia
    * @date  2013-10-15
    */
    public class TransientTest {
        
        public static void main(String[] args) {
            
            User user = new User();
            user.setUsername("Alexia");
            user.setPasswd("123456");
            
            System.out.println("read before Serializable: ");
            System.out.println("username: " + user.getUsername());
            System.err.println("password: " + user.getPasswd());
            
            try {
                ObjectOutputStream os = new ObjectOutputStream(
                        new FileOutputStream("C:/user.txt"));
                os.writeObject(user); // 将User对象写进文件
                os.flush();
                os.close();
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                // 在反序列化之前改变username的值
                User.username = "jmwang";
                
                ObjectInputStream is = new ObjectInputStream(new FileInputStream(
                        "C:/user.txt"));
                user = (User) is.readObject(); // 从流中读取User的数据
                is.close();
                
                System.out.println("\nread after Serializable: ");
                System.out.println("username: " + user.getUsername());
                System.err.println("password: " + user.getPasswd());
                
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }
        }
    }

    class User implements Serializable {
        private static final long serialVersionUID = 8294180014912103005L;  
        
        public static String username; //username 为静态变量
        private transient String passwd;
        
        public String getUsername() {
            return username;
        }
        
        public void setUsername(String username) {
            this.username = username;
        }
        
        public String getPasswd() {
            return passwd;
        }
        
        public void setPasswd(String passwd) {
            this.passwd = passwd;
        }

    }
    ```

    *Output*

    ```java
    read before Serializable: 
    username: Alexia
    password: 123456

    read after Serializable: 
    username: jmwang
    password: null
    ```

    **这说明反序列化后类中static型变量username的值为当前JVM中对应static变量的值，为修改后jmwang，而不是序列化时的值Alexia。**

4. Java中，对象的序列化可以通过实现两种接口来实现，若实现的是Serializable接口，则所有的序列化将会自动进行，若实现的是Externalizable接口，则没有任何东西可以自动序列化，需要在writeExternal方法中进行手工指定所要序列化的变量，这与是否被transient修饰无关。

    ```java
    import java.io.Externalizable;
    import java.io.File;
    import java.io.FileInputStream;
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.io.ObjectInput;
    import java.io.ObjectInputStream;
    import java.io.ObjectOutput;
    import java.io.ObjectOutputStream;

    /**
    * @descripiton Externalizable接口的使用
    * 
    * @author Alexia
    * @date 2013-10-15
    *
    */
    public class ExternalizableTest implements Externalizable {

        private transient String content = "是的，我将会被序列化，不管我是否被transient关键字修饰";

        @Override
        public void writeExternal(ObjectOutput out) throws IOException {
            out.writeObject(content);
        }

        @Override
        public void readExternal(ObjectInput in) throws IOException,
                ClassNotFoundException {
            content = (String) in.readObject();
        }

        public static void main(String[] args) throws Exception {
            
            ExternalizableTest et = new ExternalizableTest();
            ObjectOutput out = new ObjectOutputStream(new FileOutputStream(
                    new File("test")));
            out.writeObject(et);

            ObjectInput in = new ObjectInputStream(new FileInputStream(new File(
                    "test")));
            et = (ExternalizableTest) in.readObject();
            System.out.println(et.content);

            out.close();
            in.close();
        }
    }
    ```

    *Output*

    ```console
    是的，我将会被序列化，不管我是否被transient关键字修饰
    ```

## 补充：

Java序列化提供两种方式。

一种是实现Serializable接口
使用该方式非常简单，通过重写该接口预置的4个方法，可以达到对序列化的控制。不重写这些方法，也可以自动序列化，非常方便。

另一种是实现Exteranlizable接口。
不像Serializable接口只是一个标记接口，里面的接口方法都是可选的（可实现可不实现，如果不实现则启用其自动序列化功能），而Externalizable接口不是一个标记接口，它强制你自己动手实现串行化和反串行化方法。它的效率比Serializable高一些，并且可以决定哪些属性需要序列化（即使是transient修饰的），但是要求必须重写两个方法。Externalizable对小数目对象有效的多。但是对大量对象，或者重复对象，则效率低。

[Java transient关键字使用小记](https://www.cnblogs.com/lanxuezaipiao/p/3369962.html)

[Java——transient关键字及Java对象序列化](https://blog.nowcoder.net/n/fc6b0e88670340a1ac0d4ddcc79a5d68)