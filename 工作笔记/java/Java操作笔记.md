# String类为什么是Fianl的？

Fianl的意思是“最终的、最后的”，即为不再发生改变的。

Fianl可以修饰类、变量和方法，即被修饰的类、变量和方法无法发生改变。

## Fianl修饰类

### Final修饰的类无法被继承

```java
public final class StringTest {
    
}
```

![1553839900494](D:\dps\工作笔记\JAVA基础\1553839900494.png)

> 提示：被Fianl修饰的父类无法被继承！

## Final修饰变量

#### Final修饰的变量必须初始化

> #### ![](D:\dps\工作笔记\JAVA基础\1553839900494.png)

#### Final修饰的同名变量存在于父类和子类中，互不影响

```java
public class StringTest {

    public final String str = "hello";

}
```

```java
public class StringChild  extends StringTest{

    public final String str = "world";
}
```

> 可以看出在子类中存在和父类一样的Final修饰的Str，说明两个变量str是互不影响的。

#### Final修饰的类初始化

```java
public class StringTest {

    public final String str = "hello";

    public String name;
    public int age;


    public StringTest(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "StringTest{" +
                "str='" + str + '\'' +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    //省略get和Set
}
```

```java
public class StringChild  extends StringTest{

    public final StringTest stringTest = new StringTest("小明",12);
    public final String  str = "world";

    public StringChild(String name, int age) {
        super(name, age);
    }

    public static void main(String[] args) {
        System.out.println(new StringChild("小华",11).toString());
    }
}
```

`执行结果：StringTest{str='hello', name='小华', age=11}`

```java
public class StringChild  extends StringTest{

    public final StringTest stringTest = new StringTest("小明",12);
    public final String  str = "world";

    public StringChild(String name, int age) {
        super(name, age);
    }

    @Override
    public String toString() {
        return "StringChild{" +
                "stringTest=" + stringTest +
                ", str='" + str + '\'' +
                '}';
    }

    public static void main(String[] args) {
        System.out.println(new StringChild("小华",11).toString());
    }
}
```

`执行结果：StringChild{stringTest=StringTest{str='hello', name='小明', age=12}, str='world'}`

## Final修饰方法

#### 子类无法重写父类Final方法

```java
public class StringTest {

    public final void fun(){
    }
}
```

> ![1553842165530](D:\dps\工作笔记\JAVA基础\1553842165530.png)注意：子类无法重写父类的Final方法。

## 总结

#### “安全性”和“效率”

1、由于String类不能被继承，所以就不会没修改，这就避免了因为继承引起的安全隐患；

2、String类在程序中出现的频率比较高，如果为了避免安全隐患，在它每次出现时都用final来修饰，这无疑会降低程序的执行效率，所以干脆直接将其设为final一提高效率；





# @ResponseBody和@RequestBody作用

## 前言

先说明@**RequestMapping**("url")，url作为请求路径的一部分。@RequestMapping一般作用于controller的方法上作为请求的映射地址。

## @**ResponseBody**

将controller的方法返回的对象通过适当的转换器转换为指定格式后，写入到response的body区，通常用来返回json或者XML数据。需用注意的是需要**@ResponseBody**将不会再走视图处理器，而是直接将数据写入到输入流中，它的效果等同于response对象输出指定格式的数据。

## @**RequestBody**

作用在形参列表上，用于将前台数据发过来固定格式的数据（XML或JSON）封装为对象的**JAVA Bean**对象，封装时使用到的一个对象是系统默认配置的HttpMessageConverter进行解析，然后封装到形参上。



```java
	@RequestMapping("/login.do")
    @ResponseBody
    public Object login(@RequestBody User loginUuser, HttpSession session) {
        user = userService.checkLogin(loginUser);
        session.setAttribute("user", user);
        return new JsonResult(user);
    }
```

