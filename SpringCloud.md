# SpringCloud

## 包版本管理

SpringCloud也是把服务注册到nacos上的，在用户系统服务的代码中"user.api"和"user.service.impl"都依赖到了分页工具包：`pagehelper-spring-boot-starter` ，所以 **两个子项目** 的 *pom.xml* 文件都声明了同一个依赖：

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.2.13</version>
</dependency>
```

两个子项目都规定了 *pagehelper* 的版本号，带来的潜在风险是，万一升级版本，只改了其中一个子项目规定的版本号，漏改了其它的，可能会导致莫名其妙的错误。

因为一个项目运行时，同一个包只能使用一个版本，所以，万一漏改，项目发布上线后，很可能会引发不可预料的问题，这种问题往往查起来很麻烦。

为了杜绝出错，一个项目就需要统一管理依赖包的版本号，避免漏改。

## 1、处理多个子项目相同的依赖

### 1.1、修改根pom

项目 *根pom.xml* （即项目目录下的 pom.xml 文件，是所有子项目的父 pom）需要引入 `<dependencyManagement>` ，用于管理依赖包的版本号。

在 `dependencyManagement` 标签中声明所依赖的 jar 包的版本号等信息，那么所有子项目再次引入此依赖 jar 包时则无需显式的列出版本号。Maven 会沿着父子层级向上查找，直到寻找到拥有 dependencyManagement 标签的项目，然后使用它指定的版本号。

```xml
<dependencies>
... ...
</dependencies>

<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.13</version>
        </dependency>
    </dependency>
</dependencyManagement>
```

#### 注意写法：

- 原来的

   

  ```
  <dependencies>
  ```

   

  和新的

   

  ```
  <dependencyManagement>
  ```

   

  标签是并列的

  > 实际企业中的项目，不一定两种标签同时存在，每个企业习惯不同，可能只有其中一个标签

- ```
  <dependencyManagement>
  ```

   

  里只能包含一个

   

  ```
  <dependencies>
  ```

  > 注意层级关系，不能直接写`<dependency>` ，书写格式是统一的

- `<dependencies>` 可以包含多个 `<dependency>`

### 1.2、修改 user.api 的 pom

去除版本号声明，保留 `<groupId>` 和 `<artifactId>`

```xml
<dependencies>
    <dependency>
        <artifactId>user.service</artifactId>
        <groupId>com.youkeda.exercise.shared</groupId>
        <version>${project.version}</version>
    </dependency>
    <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper-spring-boot-starter</artifactId>
    </dependency>
</dependencies>
```

Maven 会根据 user.api 的 parent 溯源，直到找到 *根pom* 中的版本号声明。

### 1.3、修改 user.service.impl 的 pom

同理，user.service.impl 子项目中，也去除 `pagehelper-spring-boot-starter` 的 `<version>` 声明。

## 2、处理根项目与子项目间的重复声明

不仅是多个子项目会重复声明依赖包，有时候父项目与子项目之间也会重复声明。

例如，*根pom.xml* 与 user.service.impl/pom.xml 都声明了

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.3</version>
</dependency>
```

这是重复的。

### 2.1、修改根pom

为了统一管理，先把 `mybatis-spring-boot-starter` 的声明从原来的 `<dependencies>` ***移入*** 到 `<dependencyManagement>` 中

```xml
<dependencies>
... ...
</dependencies>

<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.13</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.3</version>
        </dependency>
    </dependency>
</dependencyManagement>
```

> 注意，移入的意思是，从原来的 dependencies 中删除。改为在 dependencyManagement 中声明

### 2.2、修改子项目pom

然后去除 user.service.impl/pom.xml 中 `mybatis-spring-boot-starter` 的 `<version>` 声明。

```xml
<dependencies>

    <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper-spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
    </dependency>

</dependencies>
```

> 子项目不再显式的列出依赖包的版本号

### 2.3、新问题

这时会发现，`user.service` 子项目报错了。

**原因：** 子项目中的 `UserService` 接口，依赖了 `org.apache.ibatis.annotations.Param` 类，这个类是 `mybatis-spring-boot-starter` 包引入的。根pom被修改，依赖声明移入 *dependencyManagement* 后就失去了依赖，因为子项目向上查找父项目声明的依赖找不到了。

> dependencyManagement 仅仅用于管理依赖包的版本号；
>
> > 与其并列的 dependencies 才表示依赖，才能在项目中使用依赖包。
> >
> > > 具体依赖了什么包，必须在 dependencies 中声明

**解决办法：** 在 user.service/pom.xml 中增加依赖声明：

```xml
<dependencies>
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
    </dependency>
</dependencies>
```

至此，`user.service` 和 `user.service.impl` 两个子项目都声明依赖了 `mybatis-spring-boot-starter` ，但版本号是根项目统一声明的。如果要升级依赖包，只需要修改一次 *根pom* 中声明的版本号，解决了修改遗漏的问题，避免可能存在的风险。

## 集成Spring Cloud Alibaba

1. 增加一个版本号变量

   根pom.xml中增加一个`spring-cloud-alibaba.version` 变量定义：

   ```xml
       <properties>
           <java.version>11</java.version>
           <!--SpringCloud Alibaba版本-->
           <spring-cloud-alibaba.version>2.2.6.RELEASE</spring-cloud-alibaba.version>
       </properties>
   ```

   定义一个版本号的好处是，有时候同一系列（groupId相同）的多个依赖包往往使用同一个版本号，可以用一个变量管理。而且 `<properties>` 一般写在 pom.xml 文件的开头，阅读时可以快速看到重要的依赖包的版本号。

   *根pom.xml* 中 **增加** 一个依赖项 `spring-cloud-alibaba-dependencies` ：

   ```xml
   <dependencyManagement>
       <dependencies>
           <dependency>
               <groupId>com.alibaba.cloud</groupId>
               <artifactId>spring-cloud-alibaba-dependencies</artifactId>
               <version>${spring-cloud-alibaba.version}</version>
               <type>pom</type>
               <scope>import</scope>
           </dependency>
       </dependencies>
   </dependencyManagement>
   ```

   > 注意引用变量的语法，变量名包含在 `${}` 中：`${spring-cloud-alibaba.version}` 。系统会自动识别变量值。

   这并不是一个依赖包，而是一个项目中包含了多个依赖包。`<type>pom</type>` 和 `<scope>import</scope>` 组合起来，表示从 `spring-cloud-alibaba-dependencies` 的 pom 文件中获取到配置的依赖包。下面会继续讲解。

    3、子项目依赖

   修改 application/pom.xml 文件， **添加** 两个依赖：

   ```xml
       <dependencies>
           <!-- Spring Cloud Begin -->
           <dependency>
               <groupId>com.alibaba.cloud</groupId>
               <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
           </dependency>
           <dependency>
               <groupId>com.alibaba.cloud</groupId>
               <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
           </dependency>
           <!-- Spring Cloud End -->
       </dependencies>
   ```

   子项目的这两个依赖包的版本号不用声明了，因为系统会从上一步的 `spring-cloud-alibaba-dependencies` 的 pom 文件中自动解析版本号。

   实际上其中包含了很多依赖包，暂时只用到两个。如果大家有兴趣，可以[点此查看官方文档](https://search.maven.org/artifact/com.alibaba.cloud/spring-cloud-alibaba-dependencies/2.2.6.RELEASE/pom)，阅读了解所有的依赖。

    4、添加注解

   添加依赖的目的，是能使用其中的类、注解等。

   修改主启动类：`Application` ，添加一个注解 `@EnableDiscoveryClient` ：

   ```java
   package com.youkeda.exercise;
   
   import org.mybatis.spring.annotation.MapperScan;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
   
   @EnableDiscoveryClient
   @SpringBootApplication(scanBasePackages = {"com.youkeda.exercise","com.youkeda.comment"})
   @MapperScan(basePackages = {"com.youkeda.comment.dao"})
   public class Application {
   
       public static void main(String[] args) {
           SpringApplication.run(Application.class, args);
       }
   
   }
   ```

   `@EnableDiscoveryClient` 是 *Spring Cloud Alibaba* 的原生注解，表示开启服务注册发现功能，也就是说，服务提供者和消费者都要使用此注解，服务注册到 nacos 上去后，才能消费者发现并调用。

   > 前面Dubbo课程中，dubbo服务也是注册到 nacos 上的。nacos作为服务注册中心，扩展性是很好的

    5、配置 nacos

   还需要配置 nacos 的地址，否则系统也不知道注册到哪里去。修改 application/src/main/resources/application.properties 配置文件，**添加**几个配置项：

   ```properties
   #application name
   spring.application.name=xxxx-user-app
   #Nacos Server ip and port
   spring.cloud.nacos.discovery.server-addr=nacos.dev.youkeda.com:8848
   ```

## Dubbo RPC-提供用户服务

Spring Cloud的服务调用方式，无论Feign还是RestTemplate都是基于Http的，而Spring Cloud Alibaba使用Nacos作为服务发现与注册，同时兼容使用Feign的Http方式和使用Dubbo的RPC方式调用。

对外部提供REST API服务是一件非常好的事情，但是如果内部调用也是使用HTTP调用方式，就会显得性能低下，Spring Cloud默认使用的Feign组件进行内部服务调用就是使用的HTTP协议进行调用，这时，我们如果内部服务使用RPC调用，对外使用REST API，将会是一个非常不错的选择

>"REST"这个词可以简单理解为：通过网址URL获取某种特定格式（例如常见的JSON）数据的方式，都可以称之为REST。这种API就叫做“REST API”或者“RESTFUL API”。
>
>

简单来说就是：内部系统之间的通信、调用，用Dubbo RPC效率更高。

### 服务提供方

使用Dubbo的RPC方式调用服务，前提是需要注册服务到Nacos

### 增加配置

修改项目配置文件

```
application/src/main/resources/application.properties
```

在Spring Cloud配置项的基础上，在新增Dubbo相关配置项：

```
#application name
spring.application.name=xxxx-user-app
#Nacos Server ip and port
spring.cloud.nacos.discovery.server-addr=nacos.dev.youkeda.com:8848

## mount to Spring Cloud
dubbo.registry.address = spring-cloud://localhost
## auto scan
dubbo.scan.base-packages = com.youkeda.comment.service.impl
dubbo.protocol.name = dubbo
dubbo.protocol.port = -1
```

`spring-cloud://localhost` 表示Dubbo 注册中心挂载到 Spring Cloud 上，也就是说，使用与 Spring Cloud 相同的配置。而且设置通信协议为 dubbo 。

> Dubbo 实际上也是支持 http 协议的，但较少使用

`dubbo.protocol.port` 用于设置 Dubbo 端口号，值为 *-1* 表示系统自动扫描可用的端口（从20880开始递增）。当我们自己的电脑同时运行服务注册、服务消费两个项目时，系统可以自动避免端口冲突，非常方便。

> dubbo 开头的配置实际上就是 Dubbo 本身的配置，也可以用于 SpringBoot + Dubbo 的项目

### user.service.impl子项目中引入依赖包

引入Dubbo相关的包

```
  <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-dubbo</artifactId>
  </dependency>
```

版本号不用写，由根pom的`spring-cloud-alibaba-dependencies`自动确定。

`spring-cloud-alibaba-dependencies` 自动确定。



### 注册服务

修改UserServiceImpl的注解

```
import org.apache.dubbo.config.annotation.DubboService;

@DubboService
public class UserServiceImpl implements UserService {
  ... ...
  ... ...
}
```



