### java 中的 @ 啥意思?

Java注解又叫java标注
编译期的东西.

### 一个完整的例子解释这件事

```java
// 等于 @Edible(value = 'xxx')
  @Edible('xxx')
  Item item = new Carrot();

  public @interface Edible {
      String value() default 'ooooo';
  }

  //此时 item.value='xxx'
```

我们测试下, 在spring例子里面:

```groovy
//还记得吗? 第一个groovy
@RestController
class HelloController {
  @Edible('xxx')
  private String hest;
  @RequestMapping("/")
  def hello() {
    return hest;
  }
}
//这个东西运行的时候, 得不到任何内容.
```

### 这个例子运行不起来

java的世界都是互相抄袭, 搞毛啊, 一点有用的东西都没有.

### 要点

1. 运行时可以用反射获得注解

   ```java
   //定义注解
   public @interface Msg {
     String DEFAULT_MSG = "msg";

     String msg() default DEFAULT_MSG;
   }

   //使用注解
   @Msg(msg = "Test")
   public class Test {

   }

   //运行时获取
   Test test = new Test();
   Class tClass = test.getClass();//只拿一个class不该new啊, 应该Test.class.
   Msg msg = (Msg) tClass.getAnnotation(Msg.class);
   System.out.println(msg.msg());

   //另外一种获取方式
   Field[] fields = Apple.class.getDeclaredFields();
   Msg fruitName = fields['msg'].getAnnotation(Msg.class);
   ```

2. 然而未必有用

   ```java
   //写成这样是不行的. 运行期报错, null指针错误.
   package com.example.readinglist;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import java.lang.annotation.*;
   @SpringBootApplication
   @RestController
   public class readingApplication {
   	@FruitName("Apple")
   	    private String appleName;	    
       @RequestMapping("/") //加了这句
      	    String home() {
   			Class tClass = readingApplication.class;
   			FruitName msg = (FruitName) tClass.getAnnotation(FruitName.class);
      	        return msg.value();
      	    }
   }
   /**
    * 水果名称注解
    */
   @Target(ElementType.FIELD) //这个是元注解, 也就是注解的注解
   @Retention(RetentionPolicy.RUNTIME) //这个也是元注解
   @Documented
   public @interface FruitName {
       String value() default " ";
   }
   ```

   ### 初步的理解

   1. 注解本身并没有任何效果.
   2. 要想某个注解生效, 需要自己写这个注解的解释器.
   3. 比如spring的注解就是spring自己弄的.
   4. 因此, 注解和反射是一对. 反射是处理注解的机制.
   5. 简单地说: 这货提供了又一个弯弯绕的机制, 真的是java风格.

### 反射

三种获得class的方式

1. Class c1 = Code.class; 这说明任何一个类都有一个隐含的静态成员变量class，这种方式是通过获取类的静态成员变量class得到的
2. Class c2 = code1.getClass(); code1是Code的一个对象，这种方式是通过一个对象的getClass()方法获得的 
3. Class c3 = Class.forName("com.trigl.reflect.Code"); 这种方法是Class类调用forName方法，通过一个类的全量限定名获得

注解用到的

1. Annotation[] annotations = (Annotation[]) class1.getAnnotations();//获取class对象的所有注解 
2. Annotation annotation = (Annotation) class1.getAnnotation(Deprecated.class);//获取class对象指定注解 
3. Type genericSuperclass = class1.getGenericSuperclass();//获取class对象的直接超类的 
4. Type Type[] interfaceTypes = class1.getGenericInterfaces();//获取class对象的所有接口的type集合

### 到了这里, 前面的疑惑解了一半了

```java
package com.example.readinglist;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import java.lang.annotation.*;

@SpringBootApplication
@RestController
public class readingApplication {
	
	@FruitName("Apple")
	    private String appleName;
	    
    @RequestMapping("/") //加了这句
   	    String home() {
			
			Class tClass = readingApplication.class;
			Annotation[] msg = (Annotation[]) tClass.getAnnotations();
			return msg.toString();
   	    }
}


/**
 * 水果名称注解
 */
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface FruitName {
    String value() default " ";
}


// 输出: [@org.springframework.boot.autoconfigure.SpringBootApplication(scanBasePackageClasses=[], exclude=[], excludeName=[], scanBasePackages=[]), @org.springframework.web.bind.annotation.RestController(value=)]
// 从这里可以看出来, 只有两个注入在类上, 其他的注入用这种方式拿不到.
```



### 元注解

> 注解的注解, 用来描述注解的作用域等等.

1. @Target  作用域
2. @Retention  生命周期
3. @Documented 描述apidoc
4. @Inherited 是否被继承
5. @Repeatable 可以用在同一个声明式或者类型加上多个相同的注解（包含不同的属性值）
6. @Native 元注解,本地方法

我们细看一下@Retention

1. RetentionPolicy.SOURCE 注解将被编译器丢弃 
2. RetentionPolicy.CLASS 注解在class文件中可用，但会被VM丢弃 
3. RetentionPolicy.RUNTIME VM将在运行期也保留注释，因此可以通过反射机制读取注解的信息

### core java volume II 10.6 source - level annotation

> 顺便吐槽下, 这个书的中文翻译真心… 看不懂啊, 最后还是看的英文.
> http://www.informit.com/articles/article.aspx?p=2027052&seqNum=6

源码级别的注解

```java
@Property String getTitle() { return title; }

@Property(editor="TitlePositionEditor")
public void setTitlePosition(int p) { titlePosition = p; }

//Property.java
package sourceAnnotations;
import java.lang.annotation.*;
@Documented
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Property
{
   String editor() default "";
}

//然后你还需要成吨的代码处理这些内容, java的注解基本可以理解为钩子, 干啥, 还要你自己写, 而且和git钩子不同的是, java的钩子好麻烦. 很不直白. 处理源码需要下面这样的命令行. 
javac -processor sourceAnnotations.BeanInfoAnnotationProcessor chart/ChartBean.java
```

> 说说我个人的看法, 我一直认为代码生成是条死路, 就不该弄这种事. 

### 总结陈词

> 这又是一个典型的java实现, 一个傻到没边的做法, 从这点看android和java真的是一脉相承. 

- 平心而论, java注解可以完成所有他需要的事.
- 但是, 和java的正常问题一样, 他的辅助性代码量太大了, 而且还很绕.
- 代码编写的过程中需要大量的copy paste, 这种编写代码的方式, 是非常痛苦, 低效, 无聊的.
- 为啥就不能类似其他语言一样简洁呢? java永远那么唠叨.

### 参考资料

1. http://www.importnew.com/10294.html
2. http://www.jianshu.com/p/a08e7e9ed765
3. http://wiki.jikexueyuan.com/project/java-reflection/java-at.html

### 相关内容: 代码生成

1. 代码生成是个糟糕的主意.
2. java目前这个情况是由于java糟糕的编译器造成的. 否则这货不该这么繁琐.
3. spring roo 是一个例子:
   - https://docs.spring.io/spring-roo/reference/html/beginning.html
   - https://projects.spring.io/spring-roo/#quick-start