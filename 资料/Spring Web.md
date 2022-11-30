### 如何把本地工程放入到课程系统中
1. 将本题的项目地址利用git，clone到本地文件夹中
![](https://style.youkeda.com/img/ham/course/j13/j13-2-1-3.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

1. 将创建好的spring工程文件放入到clone下来的文件夹中 如下图：
![](https://style.youkeda.com/img/ham/course/j13/j13-2-1-4.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

1. 完成开发之后再本地终端cd进入项目目录，然后使用git命令提交
![](https://style.youkeda.com/img/ham/course/j13/j13-2-1-7.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

1. 使用课程的终端clone本题的项目地址
![](https://style.youkeda.com/img/ham/course/j13/j13-2-1-5.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

1. 在课程中打开项目后，对代码进行**保存操作**以便提交作业
![](https://style.youkeda.com/img/ham/course/j13/j13-2-1-6.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

1. 最后点击提交作业

### Spring 的核心
依赖注入（DI）是spring最核心的技术点，spring所有技术方案都是基于DI来发展的。
### Maven入门（初）
![](https://style.youkeda.com/img/ham/course/j4/maven-logo-black-on-white.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
Apache Maven
是一个项目管理和构建自动化工具。
>Maven提供了一个命令行工具可以把工程打包成java支持的格式（比如jar），并且支持部署到中央仓库里，这样使用者只需要通过工具就可以很快捷的运用其他人写的代码，只需要你添加依赖即可

下图是一个Maven系统架构
![](https://style.youkeda.com/img/ham/course/j4/mvn.svg)

Maven使用惯例优于配置的原则。

${basedir} 存放pom.xml和所有子目录
${basedir}/pom.xml Maven的项目配置文件
${basedir}/src/main/java 项目的java源代码
${basedir}/src/main/resources 项目的资源，比如说property文件
${basedir}/src/test/java 项目的测试类，比如说JUnit代码
${basedir}/src/test/resources 测试使用的资源

这里的``${basedir}``代表的是Java工程的根路径，编辑后的classes会放在${basedir}/target/classes下面，JAR文件会放在${basedir}/target下面

### Maven命令

1. ``mvn clean compile``
编译命令，Maven会自动扫描src/main/java下的代码并完成编译工作，执行完，会在根目录下生成target/classes目录（存放所有的class）
1. ``mvn clean package``
编译并打包命令，这个命令是compile和package的集合，也就是说会先执行compile命令，然后在执行jar打包命令，这个的结果会把所有的Java文件和资源打包成一个``jar``，jar是Java的一个压缩格式，方便我们灵活的运用多个代码
1. ``mvn clean install``
执行安装命令，这个命令是compile和package、install的集合，也就是说会先执行compile命令，然后在执行jar打包命令，然后执行install命令安装到本地的Maven仓库目录里，这个目录是``${user_home}/.m2``

这个``${user_home}``指的就是你的电脑登录用户名的个人目录
1. ``mvn compile exec:java -Dexec.mainClass=${main}``
在compile执行完后，执行运行java的命令，具体执行哪个Java类是由``-Dexec.mainClass=${main}``参数指定的，比如我们想执行``com.youkeda.Test``类，那么这个完整的命令就是
```
mvn compile exec:java -Dexec.mainClass=com.youkeda.Test
```

### Maven 入门（中）
![](https://style.youkeda.com/img/ham/course/j4/Maven%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
这五个概念都会运用在Maven的配置文件中。Maven的配置文件是一个强约定的**XML**格式文件,它的文件名一定是``pom.xml``。
**1、POM(Project Object Model)**
1. Maven坐标
1. Maven工程属性
1. Maven依赖
1. Maven插件
![](https://style.youkeda.com/img/ham/course/j4/pomxml.svg)
>HTML语言也是XML格式，不过XML格式会严格遵守``标记语言``的要求，那就是有开始标签和结束标签，比如``<version>1.0</version>``

* 1.1 Maven坐标
```
<groupId>com.youkeda.course</groupId>
<artifactId>app</artifactId>
<packaging>jar</packaging>
<version>1.0-SNAPSHOT</version>
```
这四个标签组成了Maven的坐标，Maven的坐标决定了这个Maven工程部署后存在Maven仓库的文件位置。

groupid

groupid就像一个文件夹一样，比如``com.youkeda.course``。一般来说一个公司会设置自己的groupid，避免和1其他公司重合，个人开发者也一样。

artifactid

artifactid有点像文件名一样，在一个groupid内，它应该是唯一的，从规范上来说只能使用小写的英文字母、``.````-````_``。比如：``app、member.shared``

packaging

Maven工程执行完后会把整个工程打包成packaging指定的文件格式，默认情况下``packaging``的值是jar，所以如果``pom.xml``文件中没有声明这个标签，那就是jar

packaging有如下的几种格式
* jar
* war
* ear
* pom

version
在Maven的世界里，会把一个工程分为两个状态，这也是软件工程里最最常用的规范
* **SNAPSHOT**这个单词意思为快照，实际上代表了当前程序还处于不稳定的阶段，随时可以再修改，所以在外面开发的时候，我们会在版本号后面加上``SNAPSHOT``关键字
* **RELEASE**RELEASE和SNAPSHOT是对立面的，所以它代表的就是稳定，一般我们正式发布的时候，都会把version改为``RELEASE``。当然可以不用特意的加上RELEASE，因为只要不是``SNAPSHOT``,那就是``RELEASE``

在软件工程里，我们一般会用三位数字来表示版本号，所以大概是``x.x.x``这样的格式，比如说iPhone 11搭配的操作系统的版本是``iOS 13.1.2``，正如所想，这是一个RELEASE版本

三位版本号规则
* 第一位代表的是主版本号
>主版本号一般是团队约定来的，上面的iOS的例子中``13``就是主版本号

* 第二位代表的是新增功能
>上面的例子中``1``代表的就是新增功能后的版本，这个数字表明苹果在13这个版本里，有了1次新增功能的行为

* 第三位代表的是bugfix后的版本
>bugfix是修复代码缺陷、bug的行为，发布版本用于修复问题，所以上面的``2``就是bugfix版本，从这个数字你可以大致判断，苹果做了两次bug修复

在编程过程中约定大于一切

软件的版本大多时候是从``1.0.0``开始的，在开发状态下那就是``1.0.0-SNAPSHOT``,注意这个格式``[version]-SNAPSHOT``
>最后一个约定，那就是执行``mvn package、mvn install``命令生成的jar文件名是``[artifactId]-[version].jar``。如``app-1.0.0-SNAPSHOT.jar``

1.2 Maven 属性配置
```
<properties>
    <java.version>1.8</java.version>
    <maven.compiler.source>${java.version}</maven.compiler.source>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.target>${java.version}</maven.compiler.target>
</properties>
```
首先它的格式是在``properties``标签内的，这个是固定格式。properties内的标签可以自定义，但是一般来说只能是小写英文字母+``.``

* java.version
>代表设置一个参数：``java.version``,它的值是1.8

* mavencompiler.source
>这个参数是指定Maven编译时候源代码的JDK版本，``${java.version}``这个值有点特殊，它是一个动态值，``${key}``这个语法会动态找到``key``这个参数配置的值，所以上面的例子中``${java.version}``的实际值是``1.8``

* project.build.sourceEncoding
>这个参数指定的是工程代码源文件的**文件编码格式**。一般情况下，我们都设置成``UTF-8``

* maven.compiler.target
>这个参数作用是按照这个值进行编译源代码，比如这里的例子是按照JDK1.8进行编译。

### Maven下

** 1. 依赖管理 dependencies **
dependency就是用于指定当前工程依赖其他代码库的，Maven会自动管理jar依赖
>一旦我们在pom.xml里声明了dependency信息，会先去本地用户目录下的``.m2``文件夹内查找对应的文件，如果没有找到那么就会触发从中央仓库下载行为，下载完会存在本地的``.m2``文件夹内

首先声明一个父标签dependencies,然后就可以在这个标签内添加依赖了。比如我们之前学习到使用``fastjson``这个库
```
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.62</version>
    </dependency>
</dependencies>
```
一个pom.xml只能存在一个dependencies标签，可以有多个dependency，因为我们可能依赖很多库

``dependency``标签内容其实就是**Maven坐标**。
>一般我们会把别人写的代码库称为**三方库**，自己、团队写的称为**二方库**。

**1.1中央仓库**
Maven会把所有的jar文件都存放在中央仓库里，我们可以通过https://search.maven.org/这个网站来搜索jar，我们可以访问阿里云的镜像服务器https://maven.aliyun.com/mvn/search


**1.2间接依赖**
如果一个``remote``工程依赖了okhttp库，而当前工程``locale``依赖了``remote``工程，这个时候``locale``工程也会自动依赖了okhttp。

**2. 插件体系 plugins**

插件体系让Maven这个工具变得高度可定制，所以可以无缝的支撑工程化能力。不同的插件有不同的作用，比如说
```
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
        </plugin>
    </plugins>
</build>
```
这里声明的一个``maven-compiler-plugin``插件用于执行maven compile的，你会发现maven的插件其实也是存放在中央仓库的坐标，也就是一切都是jar。

### Hello Spring

Spring5的Maven坐标如下
```
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>5.2.1.RELEASE</version>
</dependency>
```
Spring强调的是面向接口编程，所以大多数情况下，Spring代码都会有接口和实现类

我们来看一个Hello World例子
```
package com.youkeda;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.ComponentScan;
import com.youkeda.service.MessageService;

/**
 * Application
 */
@ComponentScan
public class Application {

    public static void main(String[] args) {

        ApplicationContext context = new AnnotationConfigApplicationContext(Application.class);
        MessageService msgService = context.getBean(MessageService.class);
        System.out.println(msgService.getMessage());

    }
}
```
代码中用到的知识点：
1. 注解
1. Spring Bean
1. Spring 扫描
1. Spring声明周期

**面向接口的理解**

![](https://style.youkeda.com/img/ham/course/j4/springbeanvs.svg)
实际上我们的演示的例子的工作就是调用了``MessageService``实例的``getMessage()``方法。

如果我们想调用``MessageService``就可以直接从上下文获取，而不需要关心它的实现类、如何实例化实现类，从而达到了真正的解耦合。

这也是Spring最大的价值，完全的屏蔽了实现细节，让使用者可以专注于接口的定义和运用即可。

### Java注解（Annotation）

在Spring中就重度的使用了Annotation，通过运行阶段动态的获取Annotation从而完成很多自定义的行为。

![](https://style.youkeda.com/img/ham/course/j4/service1.svg)
>Annotation类里可以继续引用其他的Annotation类。所以一个Annotation是由多个Annotation组合而成的

1. Target
``java.lang.annotation.Target``自身也是一个注解，它只有一个数组属性，用于设定该注解的目标范围，比如说可以作用于类或者方法等。因为是数组，所以可以同时设定多个范围

具体可以作用的类型配置在``java.lang.annotation.ElementType``枚举类中，我们说几个常用的

* ELementType。TYPE
>可以作用于类、接口类、枚举类上

* ElementType.FIELD
>可以作用于类的属性上

* ElementType.METHOD
>可以作用于类的方法上

* ElementType.PARAMETER
>可以作用于类的参数

如果想要同时作用于类和方法上，那么你就可以
```
@Target({ElementType.TYPE,ElementType.METHOD})
```
>如果某个Annotation的Target设定为METHOD，那么你就只能在方法前面上面引用它，其他的地方都不行，其他Target值也类似于这个规则

1. 1ElementType.TYPE
```
@Service
public class MessageServiceImpl implements MessageService{
    public String getMessage(){
        return "Hello World!";
    }
}
```
1. 2ElementType.METHOD
```
public class MessageServiceImpl implements MessageService{
    @ReponseBody
    public String getMessage(){
        return "Hello World";
    }
}
```
1. 3ElementType.FIELD
```
public class MessageServiceImpl implements MessageService{
    @Autowired
    priviate WorkspaceService workspaceService;
}
```
1. 4ElementType.PARAMETER
```
public class MessageServiceImpl implements MessageService{

    public String getMessage(@RequestParam("msg")String msg){
        return "Hello "+msg;
    }
}
```
**2、 Retention**
``java.lang.annotation.Retention``自身也是一个注解，它用于声明该注解的生命周期，简单的来说就是在java编译、运行的哪个环节有效，它的值定义在``java.lang.annotation.RetentionPolicy``枚举类中，它有三个值可以选择
1. SOURCE:也就是所纯注释作用
1. CLASS：也就是在编译阶段是有效的
1. RUNTIME：在运行时有效
```
@Retention(RetentionPolicy.RUNTIME)
```
这个代码表示的就是在运行期间有效
>如果是我们自己定义的Annotation，一般我们都会设置成RUNTIME
**3、Documented**
``java.lang.annotation.Documented``自身也是一个注解，它的作用是将注解中的元素包含到JavaDoc文档中，一般情况下，都会添加这个注解的。

**4、@interface**
``@interface``就是声明当前的Java类型是Annotation，固定语法。

**5、Annotation属性**
```
String value() default "";
```
Annotation的属性有点像类的属性一样，它约定了属性的类型（这个类型是基础类型：String、boolean、int、long），和属性名称（默认名称是value，在引用的时候可以省略），default代表的是默认值。

```
import org.springframwork.stereotype.Service;

@Service
public class Demo{

}
```
注意，Annotation也是Java类，所以一样需要import的

上面的``@Service``也可以写成
```
import org.springframwork.stereotype.Service;

@Service(value="Demo")
public class Demo{

}
```
value是默认属性名称是可以缩写的，所以上面的代码等同于
```
import org.springframework.stereotype.Service;

@Service("Demo")
public class Demo {

}
```
>Annotation属性是可以有很多个的，比如下面的注解类
```
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RequestParam {

    @AliasFor("name")
    String value() default "";


    @AliasFor("value")
    String name() default "";


    boolean required() default true;

    String defaultValue() default ValueConstants.DEFAULT_NONE;
}
```
在属性的代码上，我们看到``@AliasFor("name")``这个是别名的意思，就是说这个属性用这个别名也可以访问到，所以下面的代码是一样的意思
```
@RequestParam("key")
```
```
@RequestParam(value="key")
```

```
@RequestParam(name="key")
```
具体的代码如下
```
package com.youkeda.service.impl;

import com.youkeda.service.MessageService;
import org.springframework.stereotype.Service;

@Service
public class MessageServiceImpl implements MessageService{

    public String getMessage() {
         return "Hello World!";
    }

}


```

### 静态代码块

有时候，为了让系统能够自动执行一些代码，可以采用静态代码块的方式：

```
public class Hello{
    static{
        Song song = new Song();
        ... ...
        ... ...
    }
    ... ...
}
```
静态代码块没有参数也没有返回值。只要在类代码中写``static{}``就好，系统在加载这个类的时候，会自动执行静态代码块中的代码。

之所以被称之为静态代码块，是因为，无论这个类被实例化多少次（new Hello（））静态代码块都执行一次。
>静态代码块要写在类中，而不能写在方法中

静态代码块可以有多个，位置也可以随便放。但是一般来说，推荐写在属性声明语句之后，方法声明之前。
>如果有多个，将按照在类中书写的先后顺序依次执行

```
public class Hello {
  private static Map<String, Song> songMap = new HashMap<>();

  static {
    Song song = new Song();
    song.setId("001");
    songMap.put(song.getId(), song);
  }

  public Song getSong() {

  }
}
```


### Spring Bean
IoC（inversion of Control，控制反转）容器时Spring框架最最核心的组件，没有IoC容器就没有Spring框架。
>IoC是面向对象编程中的一种设计原则，可以用来减低计算机代码间的耦合度

在Spring框架中，主要通过依赖注入（Dependency Injection，简称DI）来实现IoC

所有的Java对象都会通过IoC容器转变为Bean（Spring对象的一种称呼，以后都用Bean来表示Java对象），构成应用程序主干和由Spring IoC容器管理的对象称为Beans，Beans和它们之间的依赖关系反映在容器使用的配置元数据中。基本上所有的Bean都是由接口+实现类完成的，用户想要获取Bean的实例直接从IoC容器获取就可以，不需要关心类


![](https://style.youkeda.com/img/ham/course/j4/ioc.svg)

``org.springframework.context.ApplicationContext``接口类定义容器的对外服务，通过这个接口，我们可以轻松的从IoC容器中得到Bean对象。我们在启动Java程序的时候必须要先启动IoC容器

```
ApplicationContext context = 
    new AnnotationConfigApplicationContext("fm.douban");
```
这段代码的含义就是启动IoC容器，并且会自动加载包``fm.douban``下的Bean，只要引用了Spring注解的类都可以被加载（前提是在这个包下）

``AnnotationConfigApplicationContext``这个类的构造函数有两种
* AnnotationConfigApplicationContext(String basePackages)根据包名实例化
* AnnotationConfigApplicationContext(Class clazz)根据自定义包扫描行为实例化

Spring官方声明为Spring Bean 的注解有如下几种：
* org.springframework.stereotype.Service
* org.springframework.stereotype.Component
* org.springframework.stereotype.Controller
* org.springframework.stereotype.Repository
只要我们在类上引用这些类注解，那么都可以被IoC容器加载
*``@Component``注解是通用的Bean注解，其余三个注解都是扩展自``Component`` 
*``@Service``正如这个名称一样，代表的是Service Bean
*``@Controller``作用于Web Bean
*``@Repository``作用于持久化相关Bean

实际上这四个注解都可以被IoC容器加载，一般情况下，我们使用@Service；如果是Web应用服务就是用@Controller

在企业开发中，仅仅修改错误是不够的，更加需要的是防止出错的方法、机制。

加注解的作用，是让Spring系统自动管理各种实例
所谓**管理**，就是用@Service注解把SubjectServiceImpl和SongServiceImpl等等所有服务实现，都标记成Spring Bean；然后，在任何需要服务的地方，用@Autowired注解标记，告诉Spring这里需要注入实现类的实例

项目启动过程中，Spring会自动实例化服务实现类，然后自动注入到变量中，不需要开发者大量的写new代码了，解决了上述的开发者需要写大量代码而容易出错的问题

@Service和@Autowired是相辅相成的：如果SongServiceImpl没有加@Service，就意味着没有标记成Spring Bean ，那么即使加了@Autowired也无法注入实例；而private SongService songService;属性忘记加@Autowired,Spring Bean亦无法注入实例。二者缺一不可。
>每个Annotation都有各自特定的功能，Spring检查到代码中有注解，就自动完成特定功能，减少代码量、降低系统复杂度

初学Spring，需要理解并牢记这些常见的注解的功能和作用。


### Spring Resource
文件系统时编程不可避开的领域，因为我们总是有可能读文件、写文件。Spring Framework作为完整的Java企业级解决方案，自然也有文件处理方案，那就是``Spring Resource``
在我们正式学习Spring Resource的时候，还需要了解一下在Java工程文件的几种情况
1. 文件在电脑某个位置，比如``d:/mywork/a.doc``
1. 文件在工程目录下，比如说``mywork/toutiao.png``
1.文件在工程的``src/main/resources``目录下，我们在Maven的知识里介绍过这是Maven工程存放文件的地方
第一种和第二种情况都是使用File对象就可以读写，第三种情况比较特殊，因为Maven执行package的时候，会把resources目录下的文件一起打包进jar包里（我们之前提到过jar是java的压缩文件）。

**classpath**

在Java内部当中，我们一般把文件路径称为classpath，所以读取内部文件就是从classpath内读取，classpath指定的文件不能解析成file对象，但是可以解析成InputStream，我们借助Java IO就可以读取出来了

classpath类似于虚拟目录，它的根目录是从``/``开始代表的是``src/main/java``或者``src/main/resources``目录

我们来看一下如何使用classpath读取文件，这次我们在resources目录下存放一个data.json文件
>Java拥有很多丰富的第三方库给我们使用，读取文件，我们可以使用commons-io这个库来，需要是我们在pom.xml下添加依赖

```
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.6</version>
</dependency>
```

测试代码如下：
```
public class Test {

  public static void main(String[] args) {
    // 读取 classpath 的内容
    InputStream in = Test.class.getClassLoader().getResourceAsStream("data.json");
    // 使用 commons-io 库读取文本
    try {
      String content = IOUtils.toString(in, "utf-8");
      System.out.println(content);
    } catch (IOException e) {
      // IOUtils.toString 有可能会抛出异常，需要我们捕获一下
      e.printStackTrace();
    }
  }
}
```
再来认识一下这段代码：
```
InputStream in = Test.class.getClassLoader().getResourceAsStream("data.json");
```
这段代码的含义就是从Java运行的类加载器（ClassLoader）实例中查找文件，``Test.class``指的当前的Test.java编译后的Java class文件。


**读取一下文件内容并打印**
```
package com.youkeda;

import java.io.IOException;
import java.io.InputStream;
import org.apache.commons.io.IOUtils;

/**
 * Test
 */
public class Test {

  public static void main(String[] args) {
    // 读取 classpath 的内容
    InputStream in = Test.class.getClassLoader().getResourceAsStream("data/urls.txt");
    // 使用 commons-io 库读取文本
    try {
      String content = IOUtils.toString(in, "utf-8");
      System.out.println(content);
    } catch (IOException e) {
      // IOUtils.toString 有可能会抛出异常，需要我们捕获一下
      e.printStackTrace();
    }

  }

}
```
>了解了classpath概念后，Spring Resource能做什么呢，Spring擅长的就是封装各种服务

在Spring当中定义了一个``org.springframework.core.io.Resource``类来封装文件，这个类的优势在于可以支持普通的File也可以支持classpath文件。

并且在Spring中通过``org.springframework.core.io.ResourceLoader``服务来提供任意文件的读写，你可以在任意的Spring Bean中引入``ResourceLoader``
```
@Autowired
private ResourceLoader loader;
```
现在我们来看一下在Spring当中如何读取文件，我们创建一个自己的FileService
```
public interface FileService{
  String getContent(String name);
}
```
再看一下实现类
```
import fm.douban.service.FileService;
import org.apache.commons.io.IOUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.io.Resource;
import org.springframework.core.io.ResourceLoader;
import org.springframework.stereotype.Service;

import java.io.IOException;
import java.io.InputStream;

@Service
public class FileServiceImpl implements FileService {

    @Autowired
    private ResourceLoader loader;

    @Override
    public String getContent(String name) {
        try {
            InputStream in = loader.getResource(name).getInputStream();
            return IOUtils.toString(in,"utf-8");
        } catch (IOException e) {
           return null;
        }
    }
}
```

### Spring Bean的生命周期（Lifecycle）

![](https://style.youkeda.com/img/ham/course/j4/beaninstance.svg)
我们通过注解来声明init
```
import javax.annotation.PostConstruct;

@Service
public class SubjectServiceImpl implements SubjectService {

  @PostConstruct
  public void init(){
      System.out.println("启动啦");
  }

}  
```
我们只要在方法上添加``@PostConstruct``注解，就代表该方法在Spring Bean 启动后会自动执行的哦。

>注意这个PostConstruct的完整包路径是``javax.annotation.PostConstruct``

有了init方法后，我们可以把之前的static代码快的内容移动到init里。

```
@Service
public class SubjectServiceImpl implements SubjectService {

  @PostConstruct
  public void init(){
      Subject subject = new Subject();
      subject.setId("s001");
      subject.setName("成都");
      subject.setMusician("赵雷");
      
      subjectMap.put(subject.getId(), subject);
  }

}  
```
Spring生命周期可以让我们更轻松的初始化一些行为以及维护数据。

### Spring Boot介绍
面向微服务的框架


创建Spring Boot工程
在官方网站https://start.spring.io/可以直接创建
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/PY1/11/fm%E5%88%9B%E5%BB%BA%E5%B7%A5%E7%A8%8B.mp4)
**在本地中，只选择Spring Web即可**

![](https://style.youkeda.com/img/ham/course/j4/springboot_errorpage.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
当我们看见404错误界面，这个界面以后会经常看见，这个错误说明我们访问的服务地址是错误的，这是Spring Boot的默认错误界面，出现这个界面说明我们的应用启动成功了。


### Spring Controller入门

Spring MVC是Java Web的一种实现框架，Java Web的规范就是Servlet技术，所以所有的Java Web都实现了Servlet API的定义


Web服务器工作：
![](https://style.youkeda.com/img/ham/course/j4/springmvc1.svg)
1. Controller注解
Spring Controller本身也是一个Spring Bean，只是它提供了Web能力，我们只需要在类上提供一个@Controller注解就可以了
```
import org.springframework.stereotype.Controller;

@Controller
public class HelloControl {


}
```

1. 加载网页

在controller中，会自动加载static下的html内容

```
import org.springframework.stereotype.Controller;

@Controller
public class HelloControl {

    public String say(){
        return "hello.html";
    }

}

```

注意看say方法
* 定义返回类型为String
* ``return "hello.html"``返回的是html文件路径
>当执行这段代码的时候，Spring Boot实际加载的是``src/main/resources/static/hello.html``文件

1. RequestMapping注解

对于Web服务器来说，必须要实现的一个能力就是解析URL，并提供资源内容给调用者，这个过程一般称为**路由**
Spring MVC 完美的支持了路由能力，并简化了路由配置，只需要在提供Web访问的方法上添加一个``@RequestMapping``注解就可以完成配置。

```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloControl {

    @RequestMapping("/hello")
    public String say(){
        return "html/hello.html";
    }

}
```
### Get Request
同一个网址``https://www.baidu.com/s``,只是换了参数值，内容显示就不同了，这项技术就是URL参数解析。
![](https://style.youkeda.com/img/ham/course/j4/url.svg)

定义参数

在spring mvc中，定义一个URL参数也非常重要，我们只需要在方法上面添加对应的参数和参数注解就可以了
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
public class SongListControl {

    @RequestMapping("/songlist")
    public String index( @RequestParam("id") String id){
        return "html/songList.html";
    }

}
```

我们在之前的代码的基础上添加了
```
@RequestParam("id") String id
```
注意RequestParam注解的参数``"id"``这个值必须要和URL的param key一样，因为我们在url中定义的是id，所以这里写id

如果我们访问的URL是
```
https://域名/songlist?listId=xxxx
```
那么代码应该是
```
@RequestMapping("/songlist")
public String index( @RequestParam("listId") String id){
    return "html/songList.html";
}
```
RequestParam注解的包路径是
```
org.springframework.web.bind.annotation.RequestParam
```

由于Spring MVC的注解都是在``org.springframework.web.bind.annotation``包内，所以我们import的时候，直接用``import org.springframework.web.bind.annotation.*;``了。

在上面代码的基础上，我们添加一个分支，如果没有找到歌单就显示404界面
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
public class SongListControl {

    @RequestMapping("/songlist")
    public String index( @RequestParam("id") String id){
        if("38672180".equals(id)){
            return "html/songList.html";
        }else{
            return "html/404.html";
        }
    }
}
```
**获取多个参数**
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
public class SongListControl {

  @RequestMapping("/songlist")
  public String index(@RequestParam("id") String id,  @RequestParam("pageNum") int pageNum){
     return "html/songList.html";
  }
}
```
基础的Boolean、int、String数据类型是可以自动转化的，所以上面的页数参数使用了int，如果我们在Request方法声明了多个参数，那么URL访问的时候就必须要传递参数，要不然会访问不到。

### Get Request
@GetMapping
学习``@RequestMapping``注解用于解析URL请求路径，这个注解默认是支持所有的Http Method的。放开所有的Http Method 这样不是很安全，一般我们还是会明确制定Method，比如get请求

我们可以使用``@GetMapping``来替换注解``@RequestMapping``,它们的包路径是一样的，我们之前的代码可以写成
```
import org.springframework.web.bind.annotation.*;


@GetMapping("/songlist")
public String index(@RequestParam("id") String id,@RequestParam("pageNum") int pageNum){
  return "html/songList.html";
}
```
我们可以通过如下的URL进行访问
```
http://xxxx//songlist?id=xxx&pageNum=1
```
多个参数使用&分隔。

**非必要传递参数**

默认情况下，访问的URL中必须包含我们在Request服务里设定的参数。

如果不想某个参数必须传递，那么你可以修改一下参数的注解
```
@GetMapping("/songlist")
public String index(@RequestParam(name="pageNum",required = false) int pageNum,@RequestParam("id") String id){
  return "html/songList.html";
}
```
仔细看，应该会发现下面的代码写法
```
@RequestParam(name="pageNum",required=false)int pageNum
```
当前参数名称是pageNum，required = false表示为不是必须的，一旦我们这样设置后。下面的两个URL请求都是可以的

```
http://xxxx/songlist?id=xxx&pageNum=1

http://xxxx/songlist?id=xxx
```
>方法定义参数的顺序和URL参数的顺序没有关系。

**输出JSON数据**
我们前面的例子都是返回HTML内容，但有时候作为服务端，我们想只返回数据，Spring当中配置JSON数据如下
```
@GetMapping("/api/foos")
@ResponseBody
public String getFoos(@RequestParam("id") String id){
    return "ID: "+id;
}
```

我们调用的url是
```
https://xxxx/api/foos?id=100
```
返回Java对象的方式
```

public class User{
  private String id;
  private Stirng name;
  // 省略了 getter、setter
}


public class UserControl{
  //缓存 User 数据
  private static Map<String,User> users = new HashMap();

  /**
   * 初始化数据 
   */
  @PostConstruct
  public void init(){
    User user = new User();
    user.setId("100");
    user.setName("ykd");
    users.put(user.getId(),user);
  }
    
  @GetMapping("/api/user")
  @ResponseBody
  public User getUser(@RequestParam("id") String id) {
    return users.get(id);
  }
}
```
Spring MVC会自动的把对象转换成JSON字符串输出到网页了，一般我们会把这种输出JSON数据的方法称为API


### Thymeleaf 

Thymeleaf是一个模板框架，可以支持多种格式的内容动态渲染

**如何初始化Tymeleaf**
添加Maven依赖
```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
Spring MVC 把页面数据层封装的非常完善，只需要我们在方法参数里引入一个Model对象，就可以通过这个model对象传递数据到页面中了。

我们首先正确的导入这个Model类
```
import org.springframework.ui.Model;
```
为了更好的演示数据，我们把之前的歌单服务也集成了过来
```
@Controller
public class SongListControl {

  @Autowired
  private SongListService songListService;

  @RequestMapping("/songlist")
  public String index(@RequestParam("id")String id,Model model){

    SongList songList = songListService.get(id);
    //传递歌单对象到模板当中
    //第一个 songList 是模板中使用的变量名
    // 第二个 songList 是当前的对象实例
    model.addAttribute("songList",songList);

    return "songList";
  }
}
```
Spring MVC 中对于模板文件是有固定的存放位置，放置在工程的``src/main/resources/templates``
所以上面的``return ""songList``其实是会去查找``src/main/resources/templates/songList.html ``文件，系统会自动匹配后缀的所以不用写成"songList.html"


```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link rel="stylesheet" href="/css/songList.css" />
    <title>豆瓣歌单</title>
  </head>
  <body>
    <h1 th:text="${songList.name}"></h1>
  </body>
</html>
```
>``xmlns:th="http://www.thymeleaf.org"``的作用是，在写代码时，让软件可以识别thymeleaf语法

### Thymeleaf 变量

上一节使用的``th:text="${songList.name}"``这个就是使用的变量技术

模板变量

th:text语法的作用就是会动态换掉html标签的内部内容
```
<span th:text="${msg}">Hello</span>
```
这段代码的执行结果就是用msg变量值替换了span标签内的Hello字符串，比如说msg变量值是你好，那么代码渲染的结果是
```
<span>你好</sapn>
```

再来看看如何读取变量，上面th属性内的${msg}这个语法就是表示获取模板中的变量msg。

一般情况下，模板的变量都是会存放在模板上下文中，所以我们如果想要调用变量，就需要先设置变量到模板上下文去，你只需要像上节课一样``model.addAttribute("songList",songList);``就可以完成上下文变量的设置

比如这个例子

```
import org.springframework.ui.Model;

@Controller
public class DemoControl {

  @RequestMapping("/demo")
  public String index(Model model){
    String str = "你好";
    model.addAttribute("msg",str);
    return "demo";
  }
}
```
仔细看看``model.addAttribute("msg",str);``这个方法
* 第一个参数设置的就是上下文变量名（变量名可以随便定义）
* 第二个参数设置的是变量值（可以是任意的对象）

模板语言还可以支持复杂对象的输出，我们完全可以使用``.``把属性调用出来，比如我们的歌单对象SongList
```
import org.springframework.ui.Model;

@Controller
public class DemoControl {

  @RequestMapping("/demo")
  public String index(Model model){
    
    SongList songList = new SongList();
    songList.setId("0001");
    songList.setName("爱你一万年");

    model.addAttribute("sl",songList);
    return "demo";
  }
}
```
我们在模板中可以通过``th:text="sl.name"``、``th:text="sl.id"``分别得到name、id值
```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8" />
  </head>
  <body>
    <span th:text="${sl.id}"></span>
    <span th:text="${sl.name}"></span>
  </body>
</html>
```
### Thymeleaf 循环语句

``th:each``代表的就是循环语句

```
<ul th:each="song : ${songs}">
  <li th:text="${song.name}">歌曲名称</li>
</ul>
```
* ``${songs}``是从模板上下文获取songs这个变量
* ``song``是``${songs}``变量遍历后的每一个对象 
* ``${song.name}`` 就可以读取遍历中歌曲名称了

```
  @RequestMapping("/demo")
  public String index(Model model){
    List<Song> songs = new ArrayList<>();

    Song song = new Song();
    song.setId("0001");
    song.setName("朋友");
    songs.add(song);

    song = new Song();
    song.setId("0002");
    song.setName("夜空中最亮的星");
    songs.add(song);

    model.addAttribute("songs",songs);
    return "demo";
  }


```
**打印列表的索引值**
```
<ul th:each="song,it: ${songs}">
  <li>
    <span th:text="${it.count}"></span>
    <span th:text="${song.name}"></span>
  </li>
</ul>
```
![](https://style.youkeda.com/img/ham/course/j4/thfor.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

``th:text="song,it:${songs}"``,你会发现多了一个it，这个it是作为可选参数出行

* ``it.index``
>当前迭代对象的index（从0开始计算），如果像从0开始显示行数用这个就可以了
* ``it.count``
>当前迭代对象的index（从1开始计算）如果显示函数用这个就可以
* ``it.size``
>被迭代对象的大小，可以获取列表长度
* ``it.current``
>当前迭代变量，等同于上面的song
* ``it.even/odd``
>布尔值，当前循环是否为偶数/奇数（从0开始计算）
* ``it.first``
>布尔值，当前循环是否是第一个
* ``it.last``
>布尔值，当前循环是否是最后一个

### Thymeleaf表达式

Thymeleaf表达式主要用于两种场景
* 字符串处理
* 数据转化

**字符串处理**
我们看到的视频时间的显示效果是这样的``00:00/45:00``
在Thymealeaf中
```
<span th:text="'00:00/'+${totalTime}"></span>
```
这里需要注意的是``'00:00/'``，我们使用单引号包裹住，这个文本的作用在于把这个文本变成Java字符串，两个字符串就可以使用+拼接成新的字符串了。

```
  @RequestMapping("/demo")
  public String index(Model model){

    String totalTime = "45:00";

    model.addAttribute("totalTime",totalTime);
    return "demo";
  }
```

字符串拼接优化

我们上面的代码还可以用``|``包围住字符串，这样就不需要在文字后面附加``'...'+'...'``

```
<span th:text="|00:00/${totalTime}|"></span>
```

**数据转化**

Thymeleaf默认集成了大量的工具类可以方便的进行数据转化，一般我们使用最多的是dates

如果你想处理``LocalDate``和``LocalDateTime``类，你可以添加依赖
```
<dependency>
  <groupId>org.thymeleaf.extras</groupId>
  <artifactId>thymeleaf-extras-java8time</artifactId>
  <version>3.0.4.RELEASE</version>
</dependency>
```
这个库会自动添加一个新的工具类``temporals``

工具类的运用和变量不同，变量使用的是``${变量名}``,工具类使用的是``#{工具类}``。

dates/temporals

dates和temporals支持的方法是一样的，只是支持的类型不同，
dates支持的是Date类，temporals支持的是LocalDate和LocalDateTime

我们一般使用dates/temporals用于处理日期类型到字符串的转化，比如显示年月日

```
<p th:text="${#dates.format(dateVar,'yyyy-MM-dd')}"></p>
<p th:text="${#dates.format(dateVar,'yyyy年MM月dd日')}"></p>
```

如果显示年月日时分秒
```
<p th:text="${#dates.format(dateVar,'yyyy-MM-dd HH:mm:ss')}"></p>
<p th:text="${#dates.format(dateVar,'yyyy年MM月dd日 HH时mm分ss秒')}"></p>
```
Java代码如下
```
  @RequestMapping("/demo")
  public String index(Model model){

    Date dateVar = new Date();

    model.addAttribute("dateVar",dateVar);
    return "demo";
  }
```
如果日期类型是``LocalDate/LocalDateTime``那就把#dates换成#temporals
```
@Request("/demo")
public String index(Model model){
  LocalDateTime dateVar = LocalDateTime.now();

  model.addAttribute("dateVar",dateVar);
  return "demo";
}
```

Strings

除了日期方法，``#strings``也是我们使用比较多的，支持字符串的数据处理，比如
* ${#strings.toUpperCase(name)}
>把字符串改成全大写
* ${#strings.toLowerCase(name)}
>把字符串改为全小写
* ${#strings.arrayJoin(array,',')}
>把字符串组合并成一个字符串，并以``,``连接，比如``["a","b"]``执行后会变成``a,b``
* ${#strings.arraySplit(str,',')}
>把字符串分隔成一个数组，并以，为分隔符，比如a，b执行后会变成``["a","b"]``;如果abc没有匹配到，执行后会变成``["abc"]``
* ${#strings.trim(str)}
>把字符串去空格，左右空格都去掉
* ${#strings.length(str)}
>得到字符串的长度，也支持获取集合类的长度
* ${#strings.equals(str1,str2)}
>比较两个字符串是否相等
* ${#strings.equalsIgnoreCase(str1,str2)}
>忽略大小后比较两个字符串是否相等

更多内容请看![文档](https://www.jianshu.com/p/d8bc610c4400)

**内联表达式**

尽管我们使用``th:text``也比较方便，但是有些时候可能我们还是更喜欢直接把变量写在HTML中，比如这种写法：
```
<span>Hello [[${msg}]]</span>
```
上面的``[[变量]]``这种格式就是内联表达式，支持我们直接在HTML中调用变量
```
@RequestMapping("/demo")
public String index(Model model){
  String msg = "马婕";

  model.addAttribute("msg",msg);
  return "demo";
}
```
### Thymeleaf 条件语句

Thymeleaf也支持if/else能力，同样也是以``th:``开头的属性，这次我们使用的时``th:if``，if表达式的值是true的情况下会执行渲染
```
<span th:if="${user.sex == 'male'}">男</span>
```
配置一下Java数据
```
@RequestMapping("/demo")
public String index(Model model){
  User user = new User();
  user.setId("0001");
  user.setName("马婕");
  user.setSex("female");
  model.addAttribute("user",user);
  return "demo";
}
```
th:if条件判断除了判断Boolean值外，Thymeleaf还认为如下表达式为true：
* 值非空
* 值是非0的数字
* 值是字符串，但是不是false，off或者no
* 值不是boolean值，数字，character或字符串

用户列表

现在我们结合循环语句显示用户列表看看
```
<div>
  <li th:each="user:${users}">
    <span>[[${user.name}]]</span>
    <span th:if="${user.sex == 'famale'}">女</span>
    <span th:unless="${user.sex == 'female'}">男</span>
  </li>
</div>
```
配置Java数据如下
```
 @RequestMapping("/demo")
  public String index(Model model){

    List<User> users = new ArrayList<>();

    User user = new User();
    user.setId("0001");
    user.setName("范闲");
    user.setSex("male");
    users.add(user);

    user = new User();
    user.setId("0002");
    user.setName("司理理");
    user.setSex("female");
    users.add(user);

    user = new User();
    user.setId("0003");
    user.setName("庆帝");
    user.setSex("male");
    users.add(user);

    user = new User();
    user.setId("0004");
    user.setName("海棠朵朵");
    user.setSex("female");
    users.add(user);

    model.addAttribute("users",users);
    return "demo";
  }
```

**strings逻辑判断**
很多时候，我们还会借助``#strings``这个内置对象来做逻辑判断和数据处理，比如

isEmpty

检查字符串变量是否为空（或者为null），在检查之前会先执行trim（）操作，去掉空格
```
${#strings.isEmpty(name)}
```
数组也适用isEmpty
```
${#strings.arrayIsEmpty(name)}
```
集合类也适用于isEmpty
```
${#strings.listIsEmpty(name)}
```
我们可以测试一下
```
  @RequestMapping("/demo")
  public String index(Model model){
    String str1 = "a";
    String str2 = "";
    String str3 = "  ";
    String str4 = null;
    model.addAttribute("str1",str1);
    model.addAttribute("str2",str2);
    model.addAttribute("str3",str3);
    model.addAttribute("str4",str4);
    return "demo";
  }
```
模板代码如下
```
<p th:if="${#strings.isEmpty(str1)}">String str1 = "a";</p>
<p th:if="${#strings.isEmpty(str2)}">String str2 = "";</p>
<p th:if="${#strings.isEmpty(str3)}">String str3 = "  ";</p>
<p th:if="${#strings.isEmpty(str4)}">String str4 = null;</p>
```
运行完，大家可以看见页面只有``String str1 = "a";``没有显示除来，因为其他的几种都是匹配了isEmpty = true

**contains**

检查字符串变量是否包含片段

```
${#strings.contains(name,'abc')}
```
现在我们可以测试一下

```
@RequestMapping("/demo")
public String index(Model model){
  String str1 = "我爱马婕";

  model.addAttribute("str1",str1);
  return "demo";
}
```
模板代码如下
```
<p th:if="${#strings.contains(str1,"爱")}">匹配到爱这个字啦</p>
```
#strings判断语法还有几个常用的方法如下
* ${#strings.containsIgnoreCase(name,'abc')}
>先忽略大小写字母，然后去判断是否包含指定的字符串
* ${#strings.startsWith(name,'abc')}
>判断字符串是不是以abc开头的
* ${#strings.endsWith(name,'abc')}
>判断字符串是不是以abc结束的

#strings的字符串操作函数
* ${#strings.toUpperCase(name)}
>把字符串全部改成大写
* ${#strings.toLowerCase(name)}
>把字符串全部改成小写
* ${#strings.arrayJoin(array,',')}
>把字符串组合成一个字符串，并以，连接，比如``["a","b"]``,执行后会变成``a,b``
* ${#strings.arraySplit(str,',')},a,b["a","b"]abc["abc"]
>把字符出分隔成一个数组，并以``,``作为分隔符，比如``a,b``执行后会变成``["a","b"]``；如果abc没有匹配到，执行后会变成``["abc"]``
* ${#strings.trim(str)}
>把字符串去掉空格，左右空格都会去掉
* ${#strings.length(str)}
>得到字符串的长度，也支持获取集合类的长度
* ${#strings.equals(str1,str2)}
>比较两个字符串是否相等
* ${#strings.equalsIgnoreCase(str1,str2)
>忽略大小后比较两个字符串是否相等


### Thymeleaf 表单

```
public class Book{
  // 主键
  private long id;
  // 图书的名称
  private String name;
  // 图书的作者
  private String author;
  // 图书的描述
  private String desc;
  // 图书的编号
  private String isbn;
  // 图书的价格
  private double price;
  // 图书的封面图片
  private String pictureUrl;
  // 省略 getter、setter
}
```
页面开发
首先我们先创建一个名为``addBook.html``的thymeleaf模板
```
<form>
  <div>
    <label>书的名称:</label>
    <input type="text" />
  </div>
  <div>
    <label>书的作者:</label>
    <input type="text" />
  </div>
  <div>
    <label>书的描述:</label>
    <textarea></textarea>
  </div>
  <div>
    <label>书的编号:</label>
    <input type="text" />
  </div>
  <div>
    <label>书的价格:</label>
    <input type="text" />
  </div>
  <div>
    <label>书的封面:</label>
    <input type="text" />
  </div>
  <div>
    <button type="submit">注册</button>
  </div>
</form>
```
为了能够访问到这个页面，我们还需要配置一下Spring Control
```
package com.bookstore.control;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

@Controller
public class BookControl {
  // 当页面访问 http://localhost:8080/book/add.html 时
  // 渲染 addBook.html 模板
  @GetMapping("/book/add.html")
  public String addBookHtml(Model model){
    return "addBook";
  }
}
```
保存书籍
现在我们再新增一个control来处理书籍保存的逻辑
```
package com.bookstore.control;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

import java.util.*;

import com.bookstore.model.*;

@Controller
public class BookControl {
  //缓存所有书籍数据
  private static List<Book> books = new ArrayList<>();

  @GetMapping("/book/add.html")
  public String addBookHtml(Model model){
    return "addBook";
  }

  @PostMapping("/book/save")
  public String saveBook(Book book){
    books.add(book);
    return "saveBookSuccess";
  }

}
```
>@PostMapping 和 @GetMapping不同点在于只接收http method为post请求的数据，它的包路径和GetMapping注解类一样

我们再这个saveBook方法里面做了一个简单的处理，接收Book对象数据并存储到books对象里

我们还新增了一个templates/saveBookSuccess。html文件

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="X-UA-Compatible" content="ie=edge" />
  <title>添加书籍</title>
</head>

<body>
  <h2>添加书籍成功</h2>
</body>

</html>
```
**form表单**
我们还需要修改一下html form ，需要指定form的action属性值就是后端的请求地址，由于我们写的是/开头的，浏览器会自动把请求地址识别为``http://domain/user/reg``,如果本地开发这个domain可能是``localhost：8080``
>一般情况下，我们都会把html表单的method设置为post，这样可以保证数据传输安全，这样Spring MVC就需要接收post请求，现在我们完善代码

处理form属性调整，还需要修改input的name属性，属性和Book类的属性名要一致哦

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="X-UA-Compatible" content="ie=edge" />
  <title>添加书籍</title>
</head>

<body>
  <h2>添加书籍</h2>
  <form action="/book/save" method="POST">
    <div>
      <label>书的名称:</label>
      <input type="text" name="name">
    </div>
    <div>
      <label>书的作者:</label>
      <input type="text" name="author">
    </div>
    <div>
      <label>书的描述:</label>
      <textarea name="desc"></textarea>
    </div>
    <div>
      <label>书的编号:</label>
      <input type="text" name="isbn">
    </div>
    <div>
      <label>书的价格:</label>
      <input type="text" name="price">
    </div>
    <div>
      <label>书的封面:</label>
      <input type="text" name="pictureUrl">
    </div>
    <div>
      <button type="submit">注册</button>
    </div>
  </form>
</body>

</html>
```

### Spring Validation
我们可以使用Spring Validation 来处理表单数据的验证

**JSR 380**
JSR是Java Sprcification Requests 的缩写，意思是Java规范提案。是指向JCP（Java Community Process）提出新增一个标准化技术规范的正式请求。任何人都可以提交JSR，以向Java平台增增添新的API和服务。

JSR 380 其实就是Bean Validation 2.0，这个就是Bean验证的规范，这里的Bean就是我们一直在说的实例化后的POJO类，比如前面的Book。JSR 380 提案的规范可以通过下面的依赖添加到你的工程里
```
<dependency>
  <groupId>jakarta.validation</groupId>
  <artifactId>jakarta.validation-api</artifactId>
  <version>2.0.1</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```
Spring Validation 也是JSR 380 提案的一个实现方案

随着SpringBoot版本的不断更新，有的版本自动加入了这两个Spring Validation 的依赖包。不需要额外配置；而有的版本需要手动添加依赖

**Validation 注解**

这些注解可以直接设置在Bean属性上
* @NotNull
>不允许为null对象
* @AssertTrue
>是否为true
* @Size
>约定字符串的长度
* @Min
>字符串的最小长度
* @Max
>字符串的最大长度
* @Email
>是否为邮箱格式
* @NotEmpty
>不允许为null或者为空，可以用于判断字符串、集合、比如Map、数组、List
* @NotBlank
>不允许为null和空格

我们举一个例子，如下：
```
package com.bookstore.model;

import javax.validation.constraints.*;

public class User {

    @NotEmpty(message = "名称不能为 null")
    private String name;

    @Min(value = 18, message = "你的年龄必须大于等于18岁")
    @Max(value = 150, message = "你的年龄必须小于等于150岁")
    private int age;

    @NotEmpty(message = "邮箱必须输入")
    @Email(message = "邮箱不正确")
    private String email;
 
    // standard setters and getters 
}
```
>我们建议使用NotEmpty 替代 NotNull、NotBlank

校验的注解是可以累加的，比如上面的@Min和@Max，系统会按照顺序执行校验，任何一条校验触发就会抛出校验错误到上下文中

**创建一个表单页**
创建一个``user/addUser.html``模板文件，用于管理员添加用户

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="X-UA-Compatible" content="ie=edge" />
  <title>添加用户</title>
</head>

<body>
  <h2>添加用户</h2>
  <form action="/user/save" method="POST">
    <div>
      <label>用户名称:</label>
      <input type="text" name="name">
    </div>
    <div>
      <label>年龄:</label>
      <input type="text" name="age">
    </div>
    <div>
      <label>邮箱:</label>
      <input type="text" name="email">
    </div>
    <div>
      <button type="submit">保存</button>
    </div>
  </form>
</body>

</html>
```
**mapping**
我们还需要在``control``类里设置``mapping``
```
import javax.validation.Valid;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;
import org.springframework.validation.BindingResult;
import com.bookstore.model.*;

@Controller
public class UserControl {

  @GetMapping("/user/add.html")
  public String addUser() {
    return "user/addUser";
  }

}
```
**执行校验**
前面``addUser.html``页面中的``form action``值配置的是``/user/save``,所以我们增加一下这个请求的``Control``代码

```
package com.bookstore.control;

import com.bookstore.model.User;
import org.springframework.stereotype.Controller;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

import javax.validation.Valid;

@Controller
public class UserControl {

    @GetMapping("/user/add.html")
    public String addUser() {
        return "user/addUser";
    }

    @PostMapping("/user/save")
    public String saveUser(@Valid User user, BindingResult errors) {
        if (errors.hasErrors()) {
            // 如果校验不通过，返回用户编辑页面
            return "user/addUser";
        }
        // 校验通过，返回成功页面
        return "user/addUserSuccess";
    }

}
```

在saveUser这个方法的参数user那里，我们添加了参数注解@Valid，然后我们新增了第二个参数errors（它的类型是BindingResult）
关于@Valid（验证）的详细内容请看：![@Valid](https://blog.csdn.net/weixin_43587472/article/details/110388778?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166960480716800180635280%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166960480716800180635280&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-110388778-null-null.142^v66^wechat,201^v3^control_2,213^v2^t3_esquery_v3&utm_term=%40Valid&spm=1018.2226.3001.4187)

在链接中也包含BindingResult对象的hasErrors方法可以用于判断校验成功还是失败

**addUserSuccess.html**
```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="X-UA-Compatible" content="ie=edge" />
  <title>添加用户</title>
</head>

<body>
  <h2>添加用户成功</h2>
</body>

</html>
```




























































































