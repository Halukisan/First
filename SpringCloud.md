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

## Feign - 客户端

 Feign客户端配置和使用也很简单

在项目配置好SpringCloudAlibaba并顺利注册到Nacos后，需要做一些简单的修改

### 管理新依赖的版本

根pom的依赖包管理中，加入对SpringCloud的依赖：

```xml
 <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
```

当然，还要定义版本变量

```xml
<spring-cloud.version>Hoxton.SR6</spring-cloud.version>
```

### 开启Feign客户端

主启动类Application上加一个注解

```java
import org.springframework.cloud.openfeign.EnableFeignClients;

@EnableDiscoveryClient
@EnableFeignClients(basePackages = "com.youkeda.comment")
@SpringBootApplication(scanBasePackages = {"com.youkeda.exercise","com.youkeda.comment"})
@MapperScan(basePackages = {"com.youkeda.comment.dao"})
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

`@EnableFeignClients` 就表示是开启 *Feign* 客户端，同时，自动扫描指定的包，系统会自动实现客户端。

> 本项目自动扫描 *com.youkeda.comment*

### 子项目引入 openfeign 依赖

子项目 `comment.service`的pom.xml文件中，加一个依赖项：

```xml
<!-- openfeign -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

### 增加具体的客户端接口

评价系统要调用用户系统的API，就要在评价系统中增加一个用户接口。所以，在

子项目`comment.service`中写一个

```xml
package com.youkeda.comment.feign;

import com.youkeda.comment.model.User;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

import java.util.List;

@FeignClient(name = "xxxx-user-app")
public interface UserFeign {

    @GetMapping("/user/findByIds")
    public List<User> findByIds(@RequestParam("ids") List<Long> ids);
}
```

`UserFeign`接口相当于一个远程的API代理。`@FeignClient`注解主要用于配置远程服务的名称，表示将使用那个远程服务的API

> 即用户项目的spring.application.name指定的名称，要写自己的个性化名称。

接口中的方法，就是具体的API方法了。

`@GetMapping("/user/findByIds")`必须与远程服务control中指定的路径一致，UserFeign.findByIds()方法被调用时，其实是向用户系统发出`/user/findByIds`请求，同时附带上方法参数值。

> 所以实际上是发出了HTTP请求

UserFeign写在`comment.service`子项目中，好处是既可以给评论项目的Service使用，也可以给Control使用

### 查询用户

CommentService.impl实现类，在查询出评价后，还要同时查询评价作者信息。作者信息实际上就是User实例对象

先定义一个属性：

```java
@Component
public class CommentServiceImpl implements CommentService{
    @Autowired
    private UserFeign userFeign;
}
```

```java
List<Long> userIds = new ArrayList<>();

for (CommentDO commentDO : dos) {
    userIds.add(commentDO.getUserId());
}

List<User> userModels = userFeign.findByIds(userIds);
```

`UserFeign` 虽然是一个接口，但系统会自动构建一个代理对象实例注入，这就是面向接口编程的优点，对于一般开发者来说不用关心具体如何实现。

## 小结

构建分布式应用，步骤大概有这么几个：

1. 项目模块化划分
2. 依赖包版本管理
3. 引入依赖包
4. 完善配置（包括各种名称、地址、端口等）
5. 使用注解
6. 完成具体业务逻辑

在使用了SpringCloudAlibaba微服务架构的项目中，外面主要学习了dubbo和http两种方式的远程调用。

## Sentinel控制台

企业级的微服务架构一般比较复杂，而且企业越大，分布式环境就越复杂，于是性能、稳定性和可用性就越重要，但带来的问题是，要做好保障就越困难。

最直观的办法就是监控，随时随地了解各个服务的情况，并且可以快速处理问题。所以，服务治理是非常重要的，微服务架构体系中，除了服务之间的调用外，还需要一套服务治理的工具组件。

Sentinel是阿里开源的项目，也是SpringCloudAlibaba的一个重要组件，提供了流量控制、熔断降级、系统负载保护等多个维度来保障服务之间的稳定性。

![](https://style.youkeda.com/img/ham/course/j9/sentinel.png?x-oss-process=image/resize,w_1400/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_1,y_1)

本地安装

![点击安装](https://style.youkeda.com/img/ham/course/j9/sentinel-dashboard-1.8.2.jar)

然后打开gitbash软件，进入到程序包保存目录，然后执行

```shell
java -Dserver.port=9000 -Dcsp.sentinel.dashboard.server=localhost:9000 -Dproject.name=sentinel-dashboard -jar sentinel-dashboard-1.8.2.jar  
```

即可在本地电脑上启动 Sentinel 控制台。实际上控制台是一个 SpringBoot 项目。

> 如果有兴趣，可以研究一下 Sentinel 控制台项目：[点此打开项目](https://github.com/alibaba/Sentinel/tree/master/sentinel-dashboard)

然后在浏览器中输入 URL ：

```html
http://127.0.0.1:9000/
```

未登录时会要求登录，账号：`sentinel` ，密码：`sentinel`

即可查看控制台页面了。

![img](https://style.youkeda.com/img/ham/course/j9/j9-5-1-1.png?x-oss-process=image/resize,w_1400/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_1,y_1)

当然，还没有把项目进入到 Sentinel ，是看不到任何数据的。这里看到只是 *Sentinel控制台* 应用本身而已。

## dubbo调用-整合Sentinel

无论是服务提供者还是服务消费者，都需要接入到Sentinel，才能在Sentinel控制台上看到相关的数据。

以采用的dubbo方式调用服务为例，无论服务提供者还是消费者，步骤是一样的：

### application/pom.xml加入依赖

新增两个依赖：

```xml
 <!--SpringCloud ailibaba sentinel -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.csp</groupId>
            <artifactId>sentinel-apache-dubbo-adapter</artifactId>
        </dependency>
```

第二项依赖`sentinel-apache-dubbo-adapter` 是由于使用了 dubbo 方式调用服务而增加的。

## 2、新增配置项

application/src/main/resources/application.properties 文件新增配置项：

```properties
spring.cloud.sentinel.transport.dashboard = localhost:9000
```

此新增的配置项是指定 *Sentinel控制台* 网址。这样系统会自动发送检测数据到 *Sentinel控制台*

## 3、启动应用

暂时不需要加什么注解，只要前面两步新增依赖和配置项即可。

然后，先启动服务端应用，再启动消费端应用。必须*访问一次*消费端应用，**触发真正的服务调用**，即可在 *Sentinel控制台* 上看到监控数据啦：

![img](https://style.youkeda.com/img/ham/course/j9/j9-5-2-1.png?x-oss-process=image/resize,w_1660/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_1,y_1)

> 注意：上面图中的数值仅为演示，不用纠结为啥响应时间为 0 。

可以看到，左侧菜单栏里多了应用名称菜单，表示服务端应用已经关联了 *Sentinel控制台* ，然后点击应用下面的“实时监控”菜单项，即可看到流量曲线图。

## 名词解释

### QPS

表格中有“通过QPS”、“拒绝QPS”等几项，分别表示用户服务请求成功或失败的次数。那么什么是 *QPS* 呢？

QPS（Query Per Second）表示每秒查询次数，也就是服务器在规定时间内（秒级）所处理流量有多少。

QPS 是业界通用的衡量性能的指标之一，数值越高，说明流量（或者说请求数）越高，表示业务越繁忙。

### 响应时间

响应时间（Request Time），简称 RT

流量高不完全代表网站性能好，还要搭配“响应时间”来看。“响应时间”表示服务端请求处理完成的时间，响应时间短表示处理速度快。那么，流量高并且处理速度快，才说明服务器性能好。

*QPS* 与 *响应时间* 两个性能指标，往往是矛盾的，流量越高，服务器压力越大，响应往往越慢。

> 就像同学们的电脑打开多个 IDEA，又同时看视频、聊天，浏览器还打开了十几个网页，那么电脑就越来越卡

### 衡量性能

企业中，一般通过 *Sentinel控制台* 这样的监控工具，或者压力测试等，寻找到一个合适的 *QPS* 与 *响应时间* 值。这是每个企业积累经验的过程。监控时一旦发现与经验值偏离太大，就表示服务器可能出问题了。

> 每个企业，每个网站，由于业务复杂度、数据量、机器硬件等都各不相同，合适的 *QPS* 与 *响应时间* 值就不同。

一般来说，响应时间在 200 毫秒以内是体验比较好的，500 毫秒以内差不多也能接受，超过 800 毫秒就比较难受了。

> 当然，同学们用于学习的评价和用户系统，实际上逻辑比较简单，而且数据库中数据也不多，响应时间就很短。

## 流量控制

### 配置流控规则

启动Sentinel控制台后，浏览器输入网址并登录：

```http
http://127.0.0.1:9000/
```

> 账号：`sentinel`,密码：`sentinel`

在左侧找到用户系统菜单，然后点击“簇点链路”子菜单，找到用户服务的查询方法：

![img](https://style.youkeda.com/img/ham/course/j9/j9-5-4-3.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_1,y_1)

点击后面的“+流控”按钮。为了便于观察限流情况，可以把“单机阀值”设置的小一点，比如：5，然后点击“新增”按钮：

![img](https://style.youkeda.com/img/ham/course/j9/j9-5-4-4.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_1,y_1)

> 具体企业中的 QPS 阀值，需要咨询资深工程师

单机阀值设置为 5 ，表示超过 *5次请求/秒* 的频率就触发流控，系统自动使请求失败。

### 模拟请求

疯狂点击浏览器的刷新按钮，或者在 gitbash 中输入命令请求：

```shell
for i in {1..200} ; do curl "http://127.0.0.1:8081/comments" ; echo "" ; done
```

### 观察限流结果

点击“实时监控”菜单，回到监控页面，可以看到有些已经失败了。

![](https://style.youkeda.com/img/ham/course/j9/j9-5-4-5.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_1,y_1)

浏览器中可能出现失败：

![](https://style.youkeda.com/img/ham/course/j9/j9-5-4-6.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_1,y_1)

如果是命令请求，比较容易看到失败的响应结果：

```json
{
  "timestamp":"2021-12-02T08:54:14.211+00:00",
  "status":500,
  "error":"Internal Server Error",
  "message":"",
  "path":"/comments"
}
```

表示QPS太高了出发了限流，让某些请求失效了。

### 流量控制的意义

流控的结果，虽然一部分请求失败了，但是保护了整个系统不至于垮掉，还是有部分请求可以成功响应的。

## 熔断

一个分布式架构的系统，服务的稳定性，除了受到外部流量的影响之外，还可能受到下级服务的影响。

例如，我们在网上购物的时候发现，商品页面有展示买家的评价，那么这个服务调用链路就有更多层了：

```
商品系统
  ⇩
  ⇩（查询商品关联的评价）
  ⇩
评价服务
  ⇩
  ⇩（查询评价作者）
  ⇩
用户服务
```

如果用户服务出现问题，例如用户服务出bug，服务器硬件故障、用户数据库出问题、用户系统使用的redis出问题等，都会导致评价服务连带出问题，进而连带影响商品页面。

假设用户服务突然变得响应很慢或者不可以用，那么评价服务就更慢或者不可用，商品页面打开速度就更慢或者不可用，这种影响面逐渐放大的过程，称为**雪崩**

> 因为一个商品是有多条评价的，每条评价都要查询作者，所以影响往往是递增的。

为了防止雪崩，必须即使控制问题影响范围，评价服务必须即使停止调用用户服务。

这种即使端口某个服务调用的过程，叫做**熔断**

### 配置熔断规则

点击某个服务后面的熔断按钮

![](https://style.youkeda.com/img/ham/course/j9/j9-5-5-1.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_1,y_1)

可以在弹出框中配置熔断规则

![](https://style.youkeda.com/img/ham/course/j9/j9-5-5-2.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_1,y_1)

Sentinel提供以下几种熔断策略

* 慢调用比例（SLOW_REQUEST_RATIO）:选择以慢调用比例作为阈值，需要设置允许的慢调用RT（最大的响应时间），请求的响应时间大于该值则统计为慢调用，当单位统计时长内请求数目大于设置的最小请求数目，并且慢调用的比例大于阈值，则接下来的熔断时长内请求会自动被熔断，经过熔断时长后熔断器会进入探测恢复状态（Half-OPEN状态），若接下来的一个请求响应时间小于设置的慢调用RT则熔断结束，u肉大于设置的慢调用RT则会再次被熔断

* > RT:Response-time,响应时间，越短越好

* 异常比例（ERROR_RATIO）:当单位统计时长内请求数目大于设置的最小请求数目，并且异常的比例大于阈值，则接下来的熔断时长内请求会自动被熔断，经过熔断时长后熔断器会进入探测恢复状态（Half-open），若接下来的一个请求成功完成（没有错误）则结束熔断，否则会再次被熔断，异常比率的阈值范围是[0.0,1.0]，代表0% - 100%。

* 异常数(ERROR_COUNT)：当单位统计时长内的异常数目超过阈值之后会自动进行熔断，经过熔断时长后熔断器会进入探测恢复状态（HALF_OPEN），若接下来的一个请求成功完成（没有错误）则熔断结束，否则会被熔断。

> 三种策略简短的来讲，就是以服务调用响应时间或出错异常为衡量标准，超过阈值就执行熔断

### 熔断效果

熔断器可以在检测到下游服务出错时，不在执行调用，而是及时返回，保护上游服务和应用

> 注意，检测到出错，而不是彻底杜绝所有错误。熔断只是防止错误扩大引起雪崩

例如：评价信息中没有作者数据，商品详细页面上看不到评价作者，虽然有些瑕疵，但不至于整个商品详细页面打不开，评价系统和商品系统避免崩溃

## 降级

有时候为了改善熔断后带来的影响，可以做一些优化、补偿。

例如，如果某电商网址的商品服务挂了，那么系统熔断后，页面上看不到任何商品，这样体验很不好。这时候如果能展示一些二预置好的商品，起码页面上有充实的内容，体验得到改善。

这个熔断后的处理，叫做服务降级。

### 改造代码模拟接口故障

#### UserService接新增降级处理方法

因为评价主要调用用户服务的`findByIds()`方法，那么新增一个方法用于处理服务降级。

```jsva
List<User>findByIds(@Param("ids") List<Long> ids);
public List<User>findByUserIdsHandler(List<Long>ids,BlockException ex);
```

新增的`findByUserIdsHandler()`**熔断方法**，方法名可以自定义，自己看懂就好；方法参数在findByIds()的基础上，增加一个`BlockException ex`，当出现熔断的时候，系统自动调用熔断方法，并传入熔断异常对象，可以在方法实现中，按需要获得更多熔断异常信息。

> 方法参数`BlockException ex`是必须的，不是可写可不写

别忘记在子项目上面加上sentinel相关的依赖哦

#### UserServiceImpl服务实现类代码修改

原来的`findByids()`方法实现，加一个注解：

```java
 @Override
    @SentinelResource(value = "findUserByIds", blockHandler = "findByUserIdsHandler")
    public List<User> findByIds(@Param("ids") List<Long> ids) {

    }
```

`@SentinelResource` 注解就是表示此方法需要做降级处理。

➢ 注解参数 `value` 表示待处理的目标的名称，主要是方便 *Sentinel控制台* 上方便查看。

`value` 按照 Sentinel 官方的定义，是指资源名称。Sentinel 的资源，是一个比较宽泛的概念，可以是任何东西，服务，服务里的方法，甚至是一段代码。Sentinel 要对谁做什么操作和处理，谁就是资源。

> 资源这个概念略有点抽象，大家要仔细理解一下

➢ 注解参数 `blockHandler` 表示指定一个熔断后做降级处理的方法名。与上一步我们在 UserService 接口中新增的方法名一致。

为了模拟接口故障，`findByIds()` 做一些修改：

```java
    @Override
    @SentinelResource(value = "findUserByIds", blockHandler = "findByUserIdsHandler")
    public List<User> findByIds(@Param("ids") List<Long> ids) {
        List<User> users = new ArrayList<>();
        int b = 0;
        int a = 1;
        float c = a/b;
        return users;
    }
```

除数不能为 0 ，那么这个方法肯定会抛异常。

*再次强调：* 仅以此模拟服务方法故障，并非正常的业务代码，不要混淆了。

> 当然大家可以想其它的模拟办法

新的降级处理方法，可以返回一些默认的用户数据：

```java
    public List<User> findByUserIdsHandler(List<Long> ids, BlockException ex) {
        System.out.println("blockHandler");
        List<User> users = new ArrayList<>();

        if (CollectionUtils.isEmpty(ids)) {
            users.add(mockUser(0));
        } else {
            for (Long id : ids) {
                users.add(mockUser(id));
            }
        }

        return users;
    }

    private User mockUser(long id) {
        User user = new User();
        user.setUserName("模拟用户");
        user.setNickName("我是模拟用户");
        user.setId(id);
        return user;
    }
```

这些用代码创建的用户实例，就是返回给上游的预置数据。当数据库或 redis 等存储设备发生故障时，可以改善体验。比完全没有任何数据，要强一点。

#### 配置熔断规则

按照代码中 `@SentinelResource` 注解的 `value` 值配置熔断规则：

![img](https://style.youkeda.com/img/ham/course/j9/j9-5-6-1.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_1,y_1)

注意这里一定要按照资源名称配置熔断规则，而不是按照接口方法定义配置规则。

![img](https://style.youkeda.com/img/ham/course/j9/j9-5-6-2.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_1,y_1)

> 为了观察熔断降级，这里把异常数设置为 1 。只要出现异常就会触发熔断，实际企业中，不会这么严格，因为企业中业务逻辑和环境都很复杂，零星几次的异常是可以接受的。

当然，可以选择左侧“熔断规则”菜单，然后添加规则：

![img](https://style.youkeda.com/img/ham/course/j9/j9-5-6-3.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_1,y_1)

弹出框里的内容是一致的。

#### 本地调试

在自己电脑的开发环境上进行调试。

仍然疯狂刷新浏览器，请求：

```html
http://127.0.0.1:8081/comments
```

或者在 gitbash 中输入命令请求：

```shell
for i in {1..200} ; do curl "http://127.0.0.1:8081/comments" ; echo "" ; done
```

会发现，第一次会报错。看到错误页面或错误响应：

```shell
{
  "timestamp":"2021-12-02T08:54:14.211+00:00",
  "status":500,
  "error":"Internal Server Error",
  "message":"",
  "path":"/comments"
}
```

因为出现了一次异常，触发熔断规则，于是后面的请求会执行 `blockHandler` 指定的方法，获取到了用户，请求响应就正常了。

#### blockHandlerClass类

把熔断降级方法写在正常业务逻辑实现类（UserServiceImpl）中，代码就显得不是很优雅，毕竟是非正常的情况，写在其它类中更好。

`@SentinelResource` 支持指定类名：

```java
    @Override
    @SentinelResource(value = "findUserByIds", 
        blockHandler = "findByUserIdsHandler", 
        blockHandlerClass = UserBlockHandler.class)
    public List<User> findByIds(@Param("ids") List<Long> ids) {
        List<User> users = new ArrayList<>();
        int b = 0;
        int a = 1;
        float c = a/b;
        return users;
    }
```

`blockHandlerClass` 指定的类是自定义的。表示熔断时，降级方法会自动调用 `UserBlockHandler` 类的 `findByUserIdsHandler()` 方法。

**注意：** `findByUserIdsHandler()` 的参数写法不变，仍然是比正常方法多一个 `BlockException ex` 参数。

#### 异常回调降级

除了触发熔断规则引发服务方法调用降级以外， Sentinel 还支持服务方法调用抛异常时自动降级，回调另一个方法。这也是一种增强体验的容错机制。

```java
    @Override
    @SentinelResource(value = "findUserByIds", 
        blockHandler = "findByUserIdsHandler", 
        blockHandlerClass = UserBlockHandler.class,
        fallback = "testFallback",
        fallbackClass = TestController.class)
    public List<User> findByIds(@Param("ids") List<Long> ids) {

    }
```

`@SentinelResource` 注解的 `fallback` 指定回调方法名，`fallbackClass` 指定回调方法所在的类。本例子中，如果 `findByIds()` 抛出异常，会自动调用`TestController.testFallback()` 方法。

`fallback` 指定的回调方法，参数比正常方法多了一个 `Throwable e` ，系统就可以自动传入出现的异常实例，方便方法处理。

```java
public List<User> testFallback(List<Long> ids, Throwable e) {

}
```

因为前提触发条件不同，异常降级方法 `fallback` 和 熔断降级方法 `blockHandler` 并不矛盾，是可以共存的。实际企业中按照具体需求和习惯进行开发。

## 系统防护

除了对具体的服务进行监控和自动化处理外，还可以针对计算机系统本身做防护：

![](https://style.youkeda.com/img/ham/course/j9/j9-5-7-1.png?x-oss-process=image/resize,w_1600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_1,y_1)

系统保护规则是从应用级别的入口流量进行控制，从单台机器的load、CPU使用率、平均RT、入口QPS和并发线程数等几个维度监控应用指标，让系统尽可能跑在最大吞吐量的同时保证系统整体的稳定性。

系统保护规则是应用整体维度的，而不是资源维度的，并且仅对入口流量生效。入口流量指的是进入应用的流量，比如Web服务或者Dubbo服务端接收的请求，都属于入口流量。

### load自适应

仅对Linux/Unix-like机器生效

系统的load1作为启发指标，进行自适应系统保护。当系统load1超过设定的启发值，且系统当前的并发线程数超过估算的系统容量时才会触发系统保护（BBR阶段）。系统容量由系统的maxQps（最大秒级请求）*minRt（最小响应时间）估算得出。设定参考值一般是CPU cores（核数）*2.5.

> load是Linux操作系统中的评价CPU负载的指标，遗憾的是Windows系统不支持，购买了云服务器的可以试一下top命令即可查看到load值
>
> > load1表示近一分钟的CPU负载平均值

### CPU usage:

当系统CPU使用率超过阈值即触发系统保护（取值范围0.0-1.0），比较灵敏。

> CPU使用率和负载不是一个概念。当cpu被分配给某个任务的时候，就产生了负载；单任务不一定使用CPU的计算能力，真正使用CPU计算才统计使用率

### 平均RT：

当单台机器上所有入口流量的平均RT达到阈值即触发系统保护，单位是毫秒。

### 并发线程数

当单台机器上所有入口流量的并发线程数达到阈值即触发系统保护。

### 入口QPS：

当单台机器上所有入口流量的QPS达到阈值即触发系统保护。**同样地**，企业中需要根据实际经验确定每项指标的阈值。





