# Dubbo

一个项目同时运行在多台服务器上，称之为集群分部

分布式架构的核心理念，是按照不同领域的拆分系统，不同的子系统放在各自的项目里面，初始在各自的集群中。

![](https://style.youkeda.com/img/ham/course/j7/j7-1-1-3.svg)

拆分系统后，每个子系统的修改和上线，都是独立的，不会相互影响

1. ### 抽象核心服务

   所谓核心服务，是与具体业务关联度低，通用性高的服务。

   例如：根据用户id查询用户对象，就是核心服务。这个服务返回的用户对象，是可以用于很多场景的。无论是商品还是交易，都需要查询用户信息。

   每个子系统由web层和服务层两部分组成

   ![](https://style.youkeda.com/img/ham/course/j7/j7-1-2-2.svg)

   * web层相当于SpringBoot工程中的control，这是面向具体的业务（也就是具体的页面）提供所需的数据，依赖自身子系统和其他子系统的服务。
   * 服务层相当于SpringBoot工程中的service，提供子系统本身的核心服务。

   1.1 服务化其他优点

   ​	服务化另一个优点是解耦，每个子系统，只需要依赖其他子系统的核心服务器的接口，而不是具体的实现类。

   2. 调用核心服务

   2. ### 业务模块

      | `demo.api` |                  |
      | ---------- | ---------------- |
      | Version    | `0.0.1-SNAPSHOT` |

      ![img](https://style.youkeda.com/img/ham/course/j7/j7-2-4-1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

      > 观察一下 *demo.api/pom.xml* 与 *demo.shared/pom.xml* 比较有什么不同？

      ### 层级关系

      浓缩一下看，层级关系为：

      ```txt
      shared.all
          ↓
      demo.shared
          ↓
      demo.api
      ```

      从 *业务模块* 到 *业务父模块* 到 *业务功能模块* 。

      其中 *业务模块* 和 *业务父模块* 都是父级模块，没有 *Spring Bean* ，只是为了组织模块，整理好模块的顺序；而 *业务功能模块* 是有具体的 *Spring Bean* 实现代码的。

      所以，包含了 *Spring Bean* 的 *业务功能模块* ，其 pom.xml 中 **不能** 写 `<packaging>pom</packaging>` ，可以省略不写。

      如果你一定要写，就要写作 `<packaging>jar</packaging>` ，这也是 Maven 系统的默认值。

      - pom 用于父级工程或聚合工程，主要用于组织下级模块、控制模版版本
      - jar 用于*业务功能模块* ，系统将会打包成jar用作jar包使用，是packaging默认值。

      > `<packaging>` 不能随便写哦

      ## 重要步骤

      *demo.api* 是具体的业务功能模块，包含了代码，就需要在项目启动时被加载。但由于跟主启动类 `Application.java` 不在同一个模块中了，所以必须修改 *application/pom.xml* ，告诉 *application* 在启动的同时需要加载 *demo.api* 。

      为 *application/pom.xml* 增加依赖：

      ```java
          <dependencies>
              <dependency>
                  <artifactId>demo.api</artifactId>
                  <groupId>com.youkeda.exercise.shared</groupId>
                  <version>${project.version}</version>
              </dependency>
          </dependencies>
      ```

      `${project.version}` 是一个变量，表示同一个项目（`app.all`）中，不同的子模块之间可以 ***自动识别*** 版本号。

      *demo.api* 的 `<groupId>` 值是继承父模块的 `<groupId>`

      以后每增加一个包含代码的具体的业务子模块，都 ***必须*** 在 *application/pom.xml* 中 ***增加依赖***。否则项目启动的时候，不会自动加载的哦。

      > 以后的实战课程中可能不会再次提醒，务必牢记这个知识点。

      ## 开发HelloWorld

      *demo.api/src* 不要删除了，这是具体的业务功能模块，可以开发代码了。

      参考类图：

      ![img](https://style.youkeda.com/img/ham/course/j7/j7-2-4-2.svg)

      由于步骤较多，重点要理清楚子模块与子模块之间的关系。

      ### 要求

      - URL:

        > GET `/api/demo/hello`

      - Request Param:

        > 无

      - Response:

        > HelloWorld

      

3.1 ### 用户系统服务接口

一、 创建项目模块

* shared.all/pom.xml文件中会自动增加
* `<module>user.shared</module>`
* *user.shared/pom.xml* 中手动添加 `<relativePath>../pom.xml</relativePath>`
* *user.shared/pom.xml* 自动增加了 `<module>user.service</module>`

需要说明的是，***shared.all*** 的所有子模块都有一些共性：

- 模块版本号都是：`<version>0.0.1-SNAPSHOT</version>`
- 模块 **groupId** 都继承自父模块，实际都是 `com.youkeda.exercise.shared`

> 实际上，shared.all 及其包含的所有子模块，**groupId** 都是 *com.youkeda.exercise.shared*

### 二、迁移代码

以《深入 SSM 框架》第 7 章的代码为基础，进行重构。参考下列类图完成 *user.service* 子模块，直接把相关的代码拷贝过来即可，注意包路径保持一致，可以避免修改代码，尽量保证兼容。

![img](https://style.youkeda.com/img/ham/course/j7/j7-3-1-1.svg)

#### 拷贝操作

在创建完模块后，选中 `user.service/src/main/java` 目录，点击右键创建包路径：

![img](https://style.youkeda.com/img/ham/course/j7/j7-3-1-1-1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

然后把类文件直接拷贝过来即可。

### 三、启动项目

不要忘了非常重要的步骤，把 `user.service` 添加到 `application` 的依赖中去。这样启动的时候才能加载到此模块。

```xml
        <dependency>
            <artifactId>user.service</artifactId>
            <groupId>com.youkeda.exercise.shared</groupId>
            <version>${project.version}</version>
        </dependency>
```

## 

二、### 增加依赖

因为`UserServiceImpl` 实现了 `UserService` 接口，所以 *user.service.impl* 模块必须依赖 *user.service*。

这就要在 *user.service.impl/pom.xml* 中增加依赖。当然了，不要忘了还包括工具库、数据库等相关的依赖等。

```xml
    <dependencies>
        <!-- 其它子模块依赖 -->
        <dependency>
            <artifactId>user.service</artifactId>
            <groupId>com.youkeda.exercise.shared</groupId>
            <version>${project.version}</version>
        </dependency>
        <!-- 工具库依赖 -->
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>1.14</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.10</version>
        </dependency>
        <!-- 数据库相关的依赖 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.23</version>
        </dependency>
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
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
    </dependencies>
```

因为服务器实现和操作数据库，需要依赖到这些库，所以不能缺少哦。

## 三、迁移代码

把相关的代码和配置都拷贝到 *user.service.impl* 模块哦。

> 如果碰到编译错误，提示 `com.youkeda.comment.model.User` 不能解析或不存在，可以重启一次 *Intellj IDEA* ，让依赖关系生效。

别忘了拷贝 `UserDAO.xml` 文件哦，目录结构也是 ***对等*** 的，拷贝目标路径是： *user.service.impl/src/main/resources/com/youkeda/comment/dao/UserDAO.xml*。

在上一章第一节，已经把 `application.properties` 拷贝到 `application/src/main/resources/application.properties` 了，所以不用再拷贝。可以再检查一下数据库配置等是否有遗漏。

## 四、修改 application 依赖

由于 *user.service.impl* 依赖了 *user.service* ，所以 `application/pom.xml` 中依赖 *user.service.impl* 就可以了。

```xml
        <dependency>
            <artifactId>user.service</artifactId>
            <groupId>com.youkeda.exercise.shared</groupId>
            <version>${project.version}</version>
        </dependency>
```

***改为***

```xml
        <dependency>
            <artifactId>user.service.impl</artifactId>
            <groupId>com.youkeda.exercise.shared</groupId>
            <version>${project.version}</version>
        </dependency>
```

系统会自动解决间接依赖的问题：`application` -> `user.service.impl` -> `user.service`。`application` 启动时会自动加载两个模块的。

## 五、增加需要扫描的包

由于 `Application` 类的包路径是 `com.youkeda.exercise`，与原来的代码的包路径不一致（`com.youkeda.comment`）， `Application` 类就要使用 `scanBasePackages` 扫描相关的包。

> 这是《Spring Web全栈》第 6 章第 1 节的知识点哦

```java
@SpringBootApplication(scanBasePackages = {"com.youkeda.exercise","com.youkeda.comment"})
@MapperScan(basePackages = {"com.youkeda.comment.dao"})
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

`MapperScan` 是新知识点，用于让系统能够扫描到书写了 `SQL` 的 *xml* 文件。

这两个注解，都是解决包路径与 `Application` 启动类的包路径不一致所带来的问题。

## 六、启动项目

试试看吧，启动是否成功。应该可以看到类似的提示：

Init DruidDataSource
{dataSource-1} inited

创建模块、修改依赖、迁移代码后，按照下列检查带你回顾一下：

user.shared作为业务父模块，就包含了三个子模块

1. 检查点1：user.shared/pom.xml

```
<modules>
    <module>user.service</module>
    <module>user.service.impl</module>
    <module>user.api</module>
</modules>
```

2. 检查点2：user.api/pom..xml

   user.api只依赖user.service，不能依赖user.service.impl

   ```
   <dependencies>
       <dependency>
           <artifactId>user.service</artifactId>
           <groupId>com.youkeda.exercise.shared</groupId>
           <version>${project.version}</version>
       </dependency>
       <!-- 分页工具 -->
       <dependency>
           <groupId>com.github.pagehelper</groupId>
           <artifactId>pagehelper-spring-boot-starter</artifactId>
           <version>1.2.13</version>
       </dependency>
   </dependencies>
   ```

   这就是服务化的意义，无论是control还是api，都依赖接口，不依赖具体的实现。

   由系统自动为其注入服务实现类的示例，但从代码上，是面向接口编程的。

   3. 检查点3：application/pom.xml

   application需要启动整个项目，所以必须增加依赖user.api子模块，才能自动扫描并加载control和api

   ```
   <dependencies>
       <dependency>
           <artifactId>demo.api</artifactId>
           <groupId>com.youkeda.exercise.shared</groupId>
           <version>${project.version}</version>
       </dependency>
       <dependency>
           <artifactId>user.service.impl</artifactId>
           <groupId>com.youkeda.exercise.shared</groupId>
           <version>${project.version}</version>
       </dependency>
       <dependency>
           <artifactId>user.api</artifactId>
           <groupId>com.youkeda.exercise.shared</groupId>
           <version>${project.version}</version>
       </dependency>
   </dependencies>
   ```

   **重构点2：解决错误的依赖**

   UserController类依赖了UserDAO，这是不允许的。因为数据是非常重要的，关键的，数据库操作类DAO是不能直接开放给任意类使用的。必须通过service接口操作数据，即使service实现方法很简单，只调用一次DAO方法

   ​	2.1：UserService中增加方法

   ​		对照UserDAO，在UserService中对应的、同样定义一整套方法。但是要注意，方法参数和返回模型，都依赖User模型，而不是UserDO。例如

   ```
   public interface UserService {
     int batchAdd(@Param("list") List<User> users);
   
     List<User> findByIds(@Param("ids") List<Long> ids);
   }
   ```

   变量名也要修改，整个UserService接口不要出现UserDO字符

   ​	2.2 :模型转换DO

   ​		`UserServiceImpl` 调用 *UserDAO* 的相关写数据的方法时，需要把 *User* 模型转换为 *DO* 。所以可以增加一个 `UserDOUtil` 类来完成。

   > 可以参考 `UserDO` 的 `toModel()`，只不过是反向的。

   ```java
   package com.youkeda.comment.util;
   
   import com.youkeda.comment.dataobject.UserDO;
   import com.youkeda.comment.model.User;
   
   public class UserDOUtil {
   
       /**
        * 模型转换为 DO 对象
        *
        * @param user
        * @return
        */
       public static UserDO toDO(User user) {
           UserDO userDO = new UserDO();
           ... ...
           return userDO;
       }
   }
   ```

   这个转换方法是可以自由发挥的，也可以直接写在 `UserServiceImpl` 中，也可以换个类名。

   但需要注意的是，模型所在 *user.service* 包不能反向依赖 *user.service.impl*

   #### 2.3：完成 UserServiceImpl

   完成接口中的各个方法。例如：

   ```java
   @Override
   public int batchAdd(@Param("list") List<User> users) {
     List<UserDO> userDOs = new ArrayList<>();
   
     for (User u : users) {
       userDOs.add(UserDOUtil.toDO(u));
     }
   
     return userDAO.batchAdd(userDOs);
   }
   ```

   其实比较简单，核心就是模型转换以及调用对应的 *DAO* 方法。

   > 写数据是模型转为 *DO*；查询等读数据，就是 *DO* 转模型了。不要混淆。

   #### 2.4：解决 UserController 中的所有错误

   修改点包括：

   - 去除 `UserDAO` 和 `User` 依赖。
   - 改为依赖 `UserService`，系统自动注入其实例。
   - 改为调用 `UserService` 中的各个方法

   例如：

   ```java
   @Controller
   public class UserController {
   
     @Autowired
     private UserService userService;
   
     @GetMapping("/users")
     @ResponseBody
     public Result<Paging<User>> getAll(@RequestParam(value = "pageNum", required = false) Integer pageNum,
                                       @RequestParam(value = "pageSize", required = false) Integer pageSize) {
       Result<Paging<User>> result = new Result();
   
       if (pageNum == null) {
         pageNum = 1;
       }
       if (pageSize == null) {
         pageSize = 15;
       }
       // 设置当前页数为1，以及每页3条记录
       Page<User> page = PageHelper.startPage(pageNum, pageSize).doSelectPage(() -> userService.findAll());
   
       result.setSuccess(true);
       result.setData(new Paging<>(page.getPageNum(), page.getPageSize(), page.getPages(), page.getTotal(), page.getResult()));
       return result;
     }
   }
   ```

   至此，我们完成了用户系统的 ***模块化*** 改造。整个项目也变为 ***服务化*** 的项目。

    

### 对于作业j7-4-3-1

结构树

![image-20230817101127224](C:\Users\asd61\AppData\Roaming\Typora\typora-user-images\image-20230817101127224.png)

#### root

```xml
<modelVersion>4.0.0</modelVersion>
	<packaging>pom</packaging>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.3.0.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.youkeda.exercise</groupId>
	<artifactId>comment.all</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>comment.all</name>
	<description>评论应用</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<modules>
		<module>application</module>
		<module>shared.all</module>
	</modules>
```



#### application

包名com.youkeda.exercise

在Application中

```java
@SpringBootApplication(scanBasePackages={"com.youkeda.exercise","com.youkeda.comment"})
@MapperScan(basePackages={"com.youkeda.comment.dao"})
public class Application {

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}

}
```



在pom.xml中

```xml
 <parent>
        <artifactId>comment.all</artifactId>
        <groupId>com.youkeda.exercise</groupId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>application</artifactId>
    <version>0.0.1-SNAPSHOT</version>
```

依赖

```xml
    <dependencies>
        <dependency>
            <artifactId>comment.service.impl</artifactId>
            <groupId>com.youkeda.exercise.shared</groupId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <artifactId>comment.api</artifactId>
            <groupId>com.youkeda.exercise.shared</groupId>
            <version>${project.version}</version>
        </dependency>
    </dependencies>

```

#### shared.all

子包有comment.shared

```xml
<parent>
        <artifactId>comment.all</artifactId>
        <groupId>com.youkeda.exercise</groupId>
        <version>0.0.1-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>
    <modelVersion>4.0.0</modelVersion>
```

可以看到，它和application的这部分是一样的

因为这下面还有几个子包需要管理

所以加上

```xml
<packaging>pom</packaging>
```

然后是自己的信息

```xml
<version>0.0.1-SNAPSHOT</version>
    <groupId>com.youkeda.exercise.shared</groupId>
    <artifactId>shared.all</artifactId>
<modules>
        <module>comment.shared</module>
    </modules>
```

##### comment.shared

含有子包`comment.api` `comment.service` `comment.service.impl`

```xml
<parent>
    	<!--父组件是shared.all-->
        <artifactId>shared.all</artifactId>
        <groupId>com.youkeda.exercise.shared</groupId>
        <version>0.0.1-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>
```



然后是自己的信息

```xml
	<modelVersion>4.0.0</modelVersion>

    <artifactId>comment.shared</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
```

因为下面还有子包需要管理，所以有`<packaging>pom</packaging>`这一句

下面没有子包需要管理的，就不需要加这一句

```xml
<modules>
        <module>comment.service</module>
        <module>comment.service.impl</module>
        <module>comment.api</module>
    </modules>
```

###### comment.api

```xml
<parent>
        <artifactId>comment.shared</artifactId>
        <groupId>com.youkeda.exercise.shared</groupId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>comment.api</artifactId>
```

导入服务

```xml
<dependencies>
        <!-- 评论服务包 -->
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>comment.service</artifactId>
            <version>${project.version}</version>
        </dependency>

        <!-- 用户服务包 -->
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>user.service</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>

        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.13</version>
        </dependency>
    </dependencies>
```



###### comment.service

同样的

```xml
<parent>
        <artifactId>comment.shared</artifactId>
        <groupId>com.youkeda.exercise.shared</groupId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>comment.service</artifactId>
```

导入服务

```xml
 <dependencies>
        <!-- 用户服务包 -->
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>user.service</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
    </dependencies>
```



###### comment.service.impl

```xml
<parent>
    <artifactId>comment.shared</artifactId>
    <groupId>com.youkeda.exercise.shared</groupId>
    <version>0.0.1-SNAPSHOT</version>
</parent>
<modelVersion>4.0.0</modelVersion>

<artifactId>comment.service.impl</artifactId>
```

导入需要的依赖

```xml
<dependencies>
        <!-- 评论服务包 -->
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>comment.service</artifactId>
            <version>${project.version}</version>
        </dependency>

        <!-- 用户服务包 -->
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>user.service</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>

        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-text</artifactId>
            <version>1.8</version>
        </dependency>
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>1.14</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.10</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.23</version>
        </dependency>
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
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
    </dependencies>
```



### 引入Dubbo

#### 添加依赖

在comment.service.impl里面添加依赖

```xml
 <!-- dubbo依赖 -->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>2.7.7</version>
        </dependency>
        <!-- nacos 注册中心 -->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-registry-nacos</artifactId>
            <version>2.7.7</version>
        </dependency>
```



#### 在application里面修改配置

```properties
dubbo.application.name=youkeda-comment-app
dubbo.registry.address = nacos://nacos.dev.youkeda.com:8848
dubbo.scan.base-packages = com.youkeda.comment.service.impl
user.service.version = 1.0.0
```

## 引入Dubbo服务

`Comment` 的“作者”属性，类型是 `User` 。

也就是说，对于 `CommentService` 服务，所有 ***读*** 评论的方法，都应该返回完整的 `Comment` 实例。

所谓完整，也包括其 `author` 属性的值，也应该是完整 `User` 实例。

本节的任务就是完善 `CommentService` ，范围包括：

- `findAll()`
- `findByUserIds()`

这两个方法，调用 `DAO` 查询出来的都是 `DO` 对象，然后转换成 `Comment` 模型，但 `author` 属性的值是不完整的 `User` 实例。

所谓不完整，`CommentDO.toModel()` 只是简单的 `new User()` 并不是真正的用户服务查询到的 *User* 实例。

> 其它的 findByRefId() 方法和 query() 方法，调用 `DAO` 查询出来的实际上已经是 `Comment` 模型了
>
> > 是多表关联查询而实现的

### 思路

要查询 **评论的作者** 实例，其实就是调用 `UserService` 查询 `User` 实例。

对于 `CommentServiceImpl` 来说，只需要依赖 `UserService` ，调用其方法即可，不用关心用户服务的具体实现，不用关心 `UserServiceImpl` 的代码运行在哪个项目中。

这就是使用 *Dubbo* 的根本目的。

### 引用用户服务

用户服务是用户项目通过 *Dubbo* 暴露出来的服务，先回顾一下 *UserServiceImpl* 使用了新的注解：

```java
import org.apache.dubbo.config.annotation.DubboService;

@DubboService(version = "${user.service.version}")
public class UserServiceImpl implements UserService {

}
```

对用评论项目，`CommentServiceImpl` 依赖 `UserService` ，那么不使用一般的 `@Autowired` 注解，而是使用新的注解：

```java
import com.youkeda.comment.service.CommentService;
import org.apache.dubbo.config.annotation.DubboReference;
import org.springframework.stereotype.Component;

@Component
public class CommentServiceImpl implements CommentService {

    @DubboReference(version = "${user.service.version}")
    private UserService userService;
}
```

`@DubboReference` 表示让系统给 `userService` 注入 *Dubbo* 服务的实例。该实例肯定不是 `UserServiceImpl` 的实例，因为评论系统中，并没有 `UserServiceImpl` 的代码。

然后，*CommentServiceImpl* 的方法中，就可以正常调用 *UserService* 的各个方法了。

***注意：*** user.service.version 的值，两个项目务必保持一致，一致才能调通

### 基本原理

*Dubbo* 采用了 *Java* 代理（*proxy*）技术，是一个 *proxy* 实例。代理技术比较抽象，本节课不能花大篇幅讲详细的理论了，有兴趣的同学可以查一下资料。

代理实例的作用，可以把任何 `UserService` 的方法的调用，转发到提供服务的用户项目上，由用户项目上的真正的 `UserServiceImpl` 的实例执行完毕方法以后，再返回到调用服务的评论服务。

所以，接口就很关键了，用户项目提供了核心的 `UserService` ，不能随意修改或删除方法，方法必须 ***对等*** 才能顺利调通，否则评论服务调用时就会因为不对等而出错。

> 当然了，UserService 接口增加方法是可以的

### 相关名词

需要理解两个概念：

1. 服务提供者。即提供服务的项目。用户项目提供了 *UserService* 服务，是服务提供者。
2. 服务消费者。即依赖、使用服务的项目。评论项目使用了 *UserService* 服务，服务消费者。

> 不过有时候，程序员之间在交流的时候，*服务提供者* 和 *服务消费者* 也可以表示具体的 **类** ，而不仅仅是指大粒度的项目。
>
> > 中文博大精深，还需要分析具体的场景和语义

## 使用 Dubbo 服务的问题

了解基本原理就知道，调用服务，实际上是要经过网络传输的，加上由于参数和返回值都必须实现 `Serializable` 接口、参数和返回值都是不断进行序列化/反序列化转换的，所以，调用服务尽量不要过于频繁：

- 能一次调用批量查询数据的，不要多次调用。需要节省网络传输、序列化转换开销
- 一次调用传输数据量不要过大，尽量不要到达 *万* 级别，*千级* 以内都还好。

## 注册中心

登录到注册中心，点击左侧菜单：“服务管理” -> “订阅者列表”，就能查所有的服务消费者了。

> 订阅者就是消费者，不同叫法，同一个意思

![img](https://style.youkeda.com/img/ham/course/j7/nacos-com.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

同样，要根据个性化版本号，找到自己的服务。

## Dubbo基本原理总结

![](https://style.youkeda.com/img/ham/course/j7/j7-4-6-1.svg)

有三个阶段

#### 系统启动

注册中心是一个独立的系统，所有系统启动后都可以链接注册中心。注册中心的核心任务就是管理各种服务

Spring Boot系统在启动的时候，分别会向注册中心注册服务和订阅服务。需要注意的是：作为服务提供者系统应该先启动，否则消费者系统启动时，会因为找不到服务而出错

另外值得注意的时，评论系统实际上，也可以提供评论服务给其他系统。所以，评论系统可以既作为消费者又作为提供者，那么就不能让上游的用户系统调用评论服务，不能相互依赖，否则你就不知道应该先启动那个系统了。

### 系统运行时

消费者订阅了服务，那么提供者有什么变化，都会通知给消费者更新

### 服务调用

从图中可以看到，消费者调用提供者的服务，并不是通过注册中心中转的，而是直接请求提供者的，所以这里要求服务提供者和消费者在同一个局域网中，服务器计算机之间能够连通

> 也就是说，服务不是任意计算机都能调用的。

## 对于作业j7-6-3-1

导入了order.api

文件目录树：

![image-20230818124615764](C:\Users\asd61\AppData\Roaming\Typora\typora-user-images\image-20230818124615764.png)

### root

根组件里面pom.xml

**这里只提需要注意的事项**

首先，这里的module只能写目录下面的两个大包，不能写其他的！

```xml
<modules>
	<module>application</module>
    <module>shared.all</module>
</modules>
```

对于这个包自己

```xml
<groupId>com.youkeda.exercise</group>
<artifactId>dewu.all</artifactId>
<version>0.0.1-SNAPSHOT</version>
<name>dewu.all</name>

<properties>
	<java.version>11</java.version>
</properties>
```

注意这里的java-version写的11，但product structure里面记得改为11.

接着是添加基础的依赖，以及依赖的版本管理

```xml
<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>2.1.3</version>
		</dependency>

		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
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
			<dependency>
				<groupId>com.alibaba</groupId>
				<artifactId>druid-spring-boot-starter</artifactId>
				<version>1.1.23</version>
			</dependency>
			<dependency>
				<groupId>commons-codec</groupId>
				<artifactId>commons-codec</artifactId>
				<version>1.14</version>
			</dependency>
			<dependency>
				<groupId>org.apache.commons</groupId>
				<artifactId>commons-lang3</artifactId>
				<version>3.10</version>
			</dependency>
		</dependencies>
	</dependencyManagement>
```



### application

com.youkeda.dewu

里面只能放application这个启动类和配置

启动类里面需要添加

```java
@SpringBootApplication(scanBasePackages = {"com.youkeda.dewu"})
```

pom.xml文件里面

```xml
 <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.youkeda.exercise</groupId>
        <artifactId>dewu.all</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>

    <artifactId>application</artifactId>
    <version>0.0.1-SNAPSHOT</version>
```

接下来是依赖其他的服务或api类

```xml
<dependencies>
        <dependency>
            <artifactId>product.service.impl</artifactId>
            <groupId>com.youkeda.exercise.shared</groupId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <artifactId>product.api</artifactId>
            <groupId>com.youkeda.exercise.shared</groupId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <artifactId>order.service.impl</artifactId>
            <groupId>com.youkeda.exercise.shared</groupId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>order.api</artifactId>
            <version>${project.version}</version>
        </dependency>
    </dependencies>
```

这里只用依赖api和impl类！

### shared.all

这里简单，只需要管理好下面的子包

```xml
<modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.youkeda.exercise</groupId>
        <artifactId>dewu.all</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>

    <artifactId>shared.all</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
    <groupId>com.youkeda.exercise.shared</groupId>
    <modules>
        <module>product.shared</module>
        <module>order.shared</module>
    </modules>
```

这里的modules里面，只能有这俩子包，不要多写

#### order.shared

同样，这里的也简单，只需要管理好自己下面的包，除此之位，自己的父包也要写好,父包是上面的shared.all

这里的packaging里面的pom需要写，因为这个包下面还有其他的包，如果下面没有，就不不需要写pom

```xml
<parent>
        <groupId>com.youkeda.exercise.shared</groupId>
        <artifactId>shared.all</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>
<artifactId>order.shared</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
    <modules>
        <module>order.service</module>
        <module>order.service.impl</module>
        <module>order.api</module>
    </modules>
```

##### order.api

这里的父包是

```xml
 <parent>
        <groupId>com.youkeda.exercise.shared</groupId>
        <artifactId>order.shared</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
```

注意是order.shared

api包里面要调用各种服务，所以

```xml
 <!-- 用户服务包 -->
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>user.service</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>product.service</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>order.service</artifactId>
            <version>${project.version}</version>
        </dependency>
```

加个其他的依赖

```xml
<dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.13</version>
        </dependency>
```

##### order.service

同样的，和上面的api一样

```xml
<parent>
        <groupId>com.youkeda.exercise.shared</groupId>
        <artifactId>order.shared</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
```

这里需要用到其他的服务，订单服务需要调用到用户服务以及产品服务，所以

```xml
<dependencies>
        <dependency>
            <artifactId>product.service</artifactId>
            <groupId>com.youkeda.exercise.shared</groupId>
            <version>${project.version}</version>
        </dependency>

        <!-- 用户服务包 -->
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>user.service</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>

    </dependencies>
```

##### order.service.impl

```xml
<modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.youkeda.exercise.shared</groupId>
        <artifactId>order.shared</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>

    <artifactId>order.service.impl</artifactId>
```

服务实现类里面需要导入各种需要用到的依赖

并且，引入dubbo

```xml
<dependencies>
        <!-- dubbo依赖 -->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>2.7.7</version>
        </dependency>
        <!-- nacos 注册中心 -->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-registry-nacos</artifactId>
            <version>2.7.7</version>
        </dependency>
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>order.service</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>user.service</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>product.service</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>

        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-text</artifactId>
            <version>1.8</version>
        </dependency>
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>1.14</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.10</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.23</version>
        </dependency>
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
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <dependency>
            <groupId>org.redisson</groupId>
            <artifactId>redisson-spring-boot-starter</artifactId>
            <version>3.13.0</version>
        </dependency>
    </dependencies>
```

#### product.shared

父包是

```xml
<modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.youkeda.exercise.shared</groupId>
        <artifactId>shared.all</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>
```

对于他自己

```xml
<artifactId>product.shared</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
```

这里的packaging里面的pom需要写，因为这个包下面还有其他的包，如果下面没有，就不不需要写pom

管理下面的包

```xml
 <modules>
        <module>product.service</module>
        <module>product.api</module>
        <module>product.service.impl</module>
    </modules>
```

##### product.api

这里包括下面的service和service.impl的父包都一样

```xml
<modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.youkeda.exercise.shared</groupId>
        <artifactId>product.shared</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
```

api需要各种服务

用到那个写那个，多余的不要写

```xml
<!-- 评论服务包 -->
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>product.service</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>

        <!-- 用户服务包 -->
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>user.service</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.13</version>
        </dependency>
```

##### product.service

这里面需要用到nacos里面注册的user服务

```xml
<dependencies>
        <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>user.service</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>

    </dependencies>
```

##### product.service.impl

这里需要用到产品服务，所以需要添加对于的依赖包

```xml
 <dependency>
            <groupId>com.youkeda.exercise.shared</groupId>
            <artifactId>product.service</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
```

除此之外

```xml
<dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-text</artifactId>
            <version>1.8</version>
        </dependency>
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>1.14</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.10</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.23</version>
        </dependency>
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
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
```



































