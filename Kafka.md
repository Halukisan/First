# Kafka

`Kafka`到底是什么？它在系统中充当什么角色？它主要应用哪些领域呢？

##### 一、消息系统

Kafka 允许**发布和订阅数据**，从这点来看，它类似于 ActiveMQ、RabbitMQ 等框架，那有什么不同呢？

> Kafka 作为一个分布式系统，是以集群的方式运行的，可以自由的伸缩。同时还提供了数据传递保证 --- 可复制、持久化等等

##### 二、存储和持续处理大型数据流

`Kafka`可以存储和持续处理大型数据流，并保持持续性的低延迟。在这一点上，可以将其看成一个实时版的`Hadoop`。

> Kafka 的低延迟特点更加适合用在核心的业务应用上，当业务事件发生时，Kakfa 能够及时对这些事件作出相响应。

##### 三、实时流平台

`Kafka`其实是一个面向实时数据的流平台，也就是它不仅可以将现有的应用程序和数据系统连接起来，它还能用于加强这些触发相同数据流的应用。

> 这些数据流是现代数字科技公司的核心，与现金流一样。

将上述三个领域聚合在一起，将所有的数据流整合到一起，`Kafa`因为变得极其强大。

## kafka安装

Zookeeper

在安装Kafka之前，我们首先需要安装kafka的依赖应用zookeeper（一种分布式的，开放源码的分布式应用程序协调服务）

zookeeper对Kafka集群进行管理和负载均衡。

### Docker加速

打开阿里云网站

https://cr.console.aliyun.com/cn-hangzhou/instances/repositories

![](https://style.youkeda.com/img/course/j8/1/1-2-10.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)



> 1. 选择左下角的镜像加速器
> 2. 选择linux系统（Ubuntu，CentOS）
> 3. 拷贝图示右下区域代码

如果执行完成，没有异常则表示加速成功

### Zookeeper安装

zookeeper有现成的Docker镜像，我们只需要一键安装启动即可。

在服务器上面输入如下命令

```
sudo docker run -d --restart=unless-stopped --name zookeeper --network host zookeeper
```

启动后，可以使用`docker ps`查看zookeeper是否启动成功

### kafka安装

将私网IP和公网IP替换成如下命令对应的部分

```
sudo docker run -d --restart=unless-stopped --name kafka --network host \
          --env KAFKA_HEAP_OPTS="-Xmx256M -Xms128M" \
          --env KAFKA_ZOOKEEPER_CONNECT=127.0.0.1:2181 \
          --env KAFKA_LISTENER_SECURITY_PROTOCOL_MAP=INSIDE:PLAINTEXT,OUTSIDE:PLAINTEXT \
          --env KAFKA_LISTENERS=INSIDE://0.0.0.0:9092,OUTSIDE://0.0.0.0:9093 \
          --env KAFKA_INTER_BROKER_LISTENER_NAME=INSIDE \
          --env KAFKA_ADVERTISED_LISTENERS=INSIDE://{此处替换为私网IP}:9092,OUTSIDE://{此处替换为公网IP}:9093  \
          wurstmeister/kafka
```

注意上面命令中提示替换的地方，左右大括号也要换。

运行以后可以执行如下命令查看运行日志

```
docker logs -f kafka
```

出现如下日志表示运行成功

```
[2020-08-03 08:43:45,935] INFO [GroupMetadataManager brokerId=1004] Finished loading offsets and group metadata from __consumer_offsets-45 in 0 milliseconds. (kafka.coordinator.group.GroupMetadataManager)
[2020-08-03 08:43:45,935] INFO [GroupMetadataManager brokerId=1004] Finished loading offsets and group metadata from __consumer_offsets-48 in 0 milliseconds. (kafka.coordinator.group.GroupMetadataManager)
```

### 开放阿里云端口

和之前redis逻辑一样，我们需要在安全组里面添加9092、9093、2181端口。

## SpringBoot集成Kafka

`Kafka`环境部署，本节课我们将学习如何在 `SpringBoot` 中引入 `Kafka`。

### 引入依赖包

Maven 中有直接适配`SpringBoot`的`Kafka`依赖包，我们只需要在`pom.xml`中引入即可：

```java
<dependency>
  <groupId>org.springframework.kafka</groupId>
  <artifactId>spring-kafka</artifactId>
</dependency>
```

> SpringBoot 会自动匹配版本

### 配置 Kafka 服务器

在应用的`application.properties`中，加入`Kafka服务器`的基础配置，如下

```java
#============== kafka ===================
# 指定kafka 代理地址，可以多个
spring.kafka.bootstrap-servers={服务器公网IP地址}:9093

#=============== 生产者配置=======================

spring.kafka.producer.retries=0
spring.kafka.producer.batch-size=16384
spring.kafka.producer.buffer-memory=33554432
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer

#===============消费者配置=======================
# 指定默认消费者group id
spring.kafka.consumer.group-id=test-consumer-group
spring.kafka.consumer.auto-offset-reset=earliest
spring.kafka.consumer.enable-auto-commit=true
spring.kafka.consumer.auto-commit-interval=100
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer
```

注意，这里面大多数属性都是**生产者，消费者**的基础配置，我们不需要关心，下一章马上就会学习到。 暂时我们只需要替换其中`{服务器公网IP地址}`部分，注意大括号也要一起替换喔。

最终替换以后的结果为

```java
spring.kafka.bootstrap-servers=xxx.xxx.xxx.xxx:9093
```

### 引入测试代码

在`KafkaSender`加入基础的发送消息代码，测试 Kafka 是否配置成功。

> 此代码大家不用特别纠结，在第 2 章我们将详细介绍，此处只是用来测试 Kafka 是否配置完成即可。

```java
@Controller
@RequestMapping("kafka")
public class KafkaSender {

  @Autowired
  private KafkaTemplate<String, String> kafkaTemplate;

  //发送消息方法
  @RequestMapping("/send")
  @ResponseBody
  public String send() {
    kafkaTemplate.send("topic", "youkeda");
    return "success";
  }
}
```

运行一下

代码演示



如果控制台出现如下信息则表示`Kafka`配置成功。

```java
2020-08-05 10:20:59.775  INFO 368 --- [nio-8080-exec-3] o.a.k.clients.producer.ProducerConfig    : ProducerConfig values:
        acks = 1
        batch.size = 16384

        ...... 此处省略N行

        ssl.truststore.type = JKS
        transaction.timeout.ms = 60000
        transactional.id = null
        value.serializer = class org.apache.kafka.common.serialization.StringSerializer

2020-08-05 10:20:59.877  INFO 368 --- [nio-8080-exec-3] o.a.kafka.common.utils.AppInfoParser     : Kafka version: 2.5.0
2020-08-05 10:20:59.882  INFO 368 --- [nio-8080-exec-3] o.a.kafka.common.utils.AppInfoParser     : Kafka commitId: 66563e712b0b9f84
2020-08-05 10:20:59.882  INFO 368 --- [nio-8080-exec-3] o.a.kafka.common.utils.AppInfoParser     : Kafka startTimeMs: 1596594059876
2020-08-05 10:21:00.080  INFO 368 --- [ad | producer-1] org.apache.kafka.clients.Metadata        : [Producer clientId=producer-1] Cluster ID: D4w2I2tqSpiXTomr0ixBaw
```

#### 基本错误判断

**1. 如果出现 Timeout 超时提醒**

> 请检查阿里云安全组端口是否已经打开



## 服务间的依赖耦合

`Kafka`的第一个应用场景，发布订阅消息系统。 在学些发布订阅系统之前，我们先来看一个服务耦合问题，回顾下 P4 学习的得物项目。

**还记得当我们支付成功以后的逻辑么？**

如下图所示：

![img](https://style.youkeda.com/img/course/j8/2/2-1-4.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

我们下单调用订单服务，下单成功以后用订单号调用支付服务，支付服务唤起支付宝支付。 当支付成功以后，我们会做 3 件事情：

1. 更新支付流水状态
2. 更新订单状态
3. 更新商品购买数

#### 服务之间依赖耦合严重

由于上面的业务逻辑，导致**支付服务**对**订单服务**产生了依赖关系，而这个依赖关系在架构层次来说是不被允许的，为什么呢？

> 1. **支付服务**应该是一个底层服务，理论上只有订单依赖支付，而**支付服务不需要关心并更新订单状态**。
> 2. 假设订单系统无法访问（升级，业务变更，故障等），将导致订单状态无法被同步，并且无法自动恢复。

学习完`Dubbo`后，如果我们将系统拆分成微服务架构，这种错乱的服务之间关系将给我们的系统带来灾难式的问题。

试想一下，如果我们系统继续增强，再次加入了**日志服务**，**通知服务**。

1. 商品服务会产生浏览日志，上新通知服务。
2. 订单会产生订单操作日志，订单状态变更通知。
3. 支付会产生支付记录日志，支付通知。

那么系统服务之间关系将会是什么样呢？将产生更加复杂的依赖关系，如下图所示：

![img](https://style.youkeda.com/img/course/j8/2/2-1-2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

那什么办法解除服务之间的耦合关系呢？

#### 消息系统

对，我们需要使用一个**消息系统**，利用消息解除服务之间的依赖关系。最终关系如下

![img](https://style.youkeda.com/img/course/j8/2/2-1-3.svg)

那么问题来了，消息系统如何实现呢？请看下一节。

## 发布订阅消息系统



消息系统接收订阅者订阅主题

消息系统将主题发送给该主题的订阅者

![](https://style.youkeda.com/img/course/j8/2/2-2-3.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)







## kafka生产者-写入数据

发布和订阅是两个非常重要的因素。所谓的发布，`Kafka`中被称为生产者，顾名思义生产数据，也就是往`Kafka`中写入数据。

本节课，我们将从 Kafka 生产者的设计和组件讲起，学习如何在 SpringBoot 中使用 Kafka 生产者。

我们依次分析 kafka 消息和发送机制。

#### 消息主题和内容

通过上一节的分析，我们知道每个消息都有一个明确的**主题**，用来筛选消息的订阅者。除了主题，更重要的当然是消息的**内容**。

在 Java 中，Kafka 消息用类`ProducerRecord<K, V>`表示。

> 注意这里使用了泛型，这里的 V 即是消息的内容。

有同学肯定有疑问？

**这里的 K 代表什么呢？**

> 这里的 Key 不是主题，大家可以将其当做消息的附加信息，具体作用稍后就能明白。

那么到目前为止，我们知道一个消息体的大致结果如下

![img](https://style.youkeda.com/img/course/j8/2/2-3-1.svg)

#### 内容序列化

我们知道为了网络传输，通常我们需要将内容进行序列化，同样`Kafka`也是如此，需要分别将`Key`, `Value`进行序列化。 回顾下之前`application.properties`中的配置属性。

```java
spring.kafka.producer.retries=0
spring.kafka.producer.batch-size=16384
spring.kafka.producer.buffer-memory=33554432
//#1. key序列化
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
//#2. value序列化
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer
```

注意`#1, #2`处，从配置可以看出`key`、`value`都是用的字符串序列化方式。 同样也约定我们的消息体，应该是`ProducerRecord<String, String>`类型。

> 当然 Kafka 还提供了**整数**和**字节数组**序列化器 甚至 Kafka 还提供了自定义序列化器作为扩展方案 本课程中我们暂不涉及这些复杂方案，有兴趣的同学下来自己了解下

继续在我们流程中添加序列化器

![img](https://style.youkeda.com/img/course/j8/2/2-3-2.svg)

#### 主题分区

我们继续思考的，对于消息应该用什么数据结构存储呢？

> 消息肯定满足是先入先出的规则，所以最好使用队列进行存储，俗称**消息队列**。

那么消息传递的过程如下所示：

![img](https://style.youkeda.com/img/course/j8/2/2-3-3.svg)

左侧是消息入口，右侧是消息出口，越早的消息肯定越早被消费。

但是，如果仅仅是这么简单的系统，我们自己的也可以完成，人人都能造一个 Kafka 了？事实是这样么？肯定不是。

我们知道 Kafka 是为了应对大数据量，大批量消息而设计的，这种简单的系统是肯定不支持大量并发的。在此基础上，系统需要支持**横向扩展的能力**。

Kafka 如何实现呢？

> 它提出了分区（Partition）的概念，每个分区都是一个队列，每个消息会按照一定的规则放置在某个分区里面。

我们来看一个完整的流程：

![img](https://style.youkeda.com/img/course/j8/2/2-3-4.svg)

当消息通过序列化到分区器时，系统首先根据`Topic`寻到到对应的主题区域，然后再通过规则找到对应主题下的分区。

> 默认情况下消息会被**随机**发送到主题内各个可用的分区上，并且通过算法保证分区**消息量均衡**。
>
> 如果消息体中有`Key`，则会**根据`Key`的哈希值找到某个固定分区**，也就是如果`Key`相同则分区也将相同。

## kafka生产者-Spring Boot中使用

```java
package com.example.demo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.kafka.core.KafkaTemplate;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("kafka")
public class KafkaSender {

  // #1. 定义Kafka消息类型
  @Autowired
  private KafkaTemplate<String, String> kafkaTemplate;

  //发送消息方法
  @RequestMapping("/send")
  @ResponseBody
  public String send() {
    // #2. 发送Kafka消息
    kafkaTemplate.send("topic", "youkeda");
    return "success";
  }
}
```

连接kafka

这里需要和`application.properties`中配置的序列化器相对应哈，如下所示：

![img](https://style.youkeda.com/img/course/j8/2/2-3-5.svg)

#### 发送消息

如上所示`#2`处的注释，我们用一句话即完成了消息的发送。

在这里我们使用的 `key = null` 的发送方法：

![img](https://style.youkeda.com/img/course/j8/2/2-3-6.svg)

`KafkaTemplate`同样提供了`Key != null`的发送方法：

![img](https://style.youkeda.com/img/course/j8/2/2-3-7.svg)

大家根据场景各取所需。

运行一下上面的代码如下：

代码演示



可以看到在调用`kafka/send`接口之后，控制台出现如下信息：

```java
2020-08-05 16:46:36.253  INFO 368 --- [nio-8080-exec-3] o.a.kafka.common.utils.AppInfoParser     : Kafka version: 2.5.0
2020-08-05 16:46:36.254  INFO 368 --- [nio-8080-exec-3] o.a.kafka.common.utils.AppInfoParser     : Kafka commitId: 66563e712b0b9f84
2020-08-05 16:46:36.254  INFO 368 --- [nio-8080-exec-3] o.a.kafka.common.utils.AppInfoParser     : Kafka startTimeMs: 1596617196252
2020-08-05 16:46:36.462  INFO 368 --- [ad | producer-1] org.apache.kafka.clients.Metadata        : [Producer clientId=producer-1] Cluster ID: D4w2I2tqSpiXTomr0ixBaw
```

从日志可以看到消息已经发送成功，并且返回给我们commitId。

## kafka生产者-消费数据

kafka消息消费者`KafkaConsumer`。

### 消费者 和 消费者组

从订阅发布模式来看，消费者肯定是对某个主题进行监听，一旦有消息则消费此消息，然后处理自己的业务逻辑。

试想一个场景，当消息生产的过快，消费速度跟不上生产速度会出现什么现象？这时候应该怎么办呢？

**显然对于消费者，也需要支持横向扩展的能力。**

> 就像多个生产者可以向相同的主题中写入消息一样，多个消费者也可以从同一个主题中读取消息，从而达到对消息进行分流。

那么问题来了：**不同消费者如何分配同一主题的消息呢，同一主题的消息不同分区如何分流呢**？

在这里我们需要引出一个新的概念 --- **消费者组**。

> 一个消费者组里的消费者订阅同一个主题，每个消费者接受主题一部分分区的消息。

我们举几个简单例子就能明白其中的道理

#### 案例 1：单消费者

如果一个消费者组只有一个消费者，它将消费这个主题下所有分区的消息，如下图所示：

![img](https://style.youkeda.com/img/course/j8/2/2-5-1.svg)

#### 案例 2：多消费者

如果一个消费者组有多个消费者（但不超过分区数量），它将均衡分流所有分区的消息:

![img](https://style.youkeda.com/img/course/j8/2/2-5-2.svg)

如果消费者数量和分区数量刚好相同，则每个消费者接收一个分区的消息：

![img](https://style.youkeda.com/img/course/j8/2/2-5-3.svg)

> 注意：**一条消息只会被同组消费一次**，也就是消息不会在同一个消费者组中重复消费，具有排他性。
>
> > 已经收到消息的消费者重启计算机后也不会再次接收到同一条消息。

#### 案例 3：超消费者

如果消费者数量超过分区数量怎么办呢？那么一部分消费者将会闲置，不会接受任何消息。

![img](https://style.youkeda.com/img/course/j8/2/2-5-4.svg)

比如图中的 **消费者 5** 将会闲置。

#### 案例 4： 多消费者组

在上面的例子中，如果新增**消费者组 B**，同样订阅了该主题，将会出现什么情况呢？

> 注意：**多个消费者组订阅同一个主题，将分别消费这个主题的消息**，也就是一个消息都会通知每个消费者组。

如下图所示：

![img](https://style.youkeda.com/img/course/j8/2/2-5-5.svg)



## kafka消费者-SpringBoot中使用

首先我们需要配置**消费者组，消息体Key，Value的反序列化器**，如`application.properties`中配置所示：

```java
// 配置消费者组
spring.kafka.consumer.group-id=test-consumer-group
spring.kafka.consumer.auto-offset-reset=earliest
spring.kafka.consumer.enable-auto-commit=true
spring.kafka.consumer.auto-commit-interval=100
// 配置消息体Key反序列化器
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
// 配置消息体Value反序列化器
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer
```

### 消费者代码实现

紧接着，我们完成消息接受代码，如下：

```java
package com.youkeda.demo;

import java.util.Optional;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.springframework.kafka.annotation.KafkaListener;
import org.springframework.stereotype.Component;

@Component
public class KafkaReceiver {

  // #1. 监听主题为topic的消息
  @KafkaListener(topics = {"topic"})
  public void listen(ConsumerRecord<?, ?> record) {
    // #2. 如果消息存在
    Optional<?> kafkaMessage = Optional.ofNullable(record.value());
    if (kafkaMessage.isPresent()) {
      // #3. 获取消息
      Object message = kafkaMessage.get();
      System.out.println("message =" + message);
    }

  }
}
```

注意上面 3 个注释

1. `#1` 需要利用注解的方式注册我们希望监听的 Kafka 消息主题，一旦有消息，将触发这个`listen`方法。

2. `#2` 判断 kafka 消息是否存在

   `Optional` 是 Java8 的工具类，**主要用于解决空指针异常**的问题。它提供很多有用的方法，这样我们就不用显式进行空值检测。这里主要用到三个常用的方法，以判断消息是否存在，如果存在则取出消息值。

3. `#3` 获取 Kafka 消息中的消息体



我们首先利用`kafka/send`接口发送一个消息，然后`KafkaReceiver`中将接收到刚才发出的消息。

运行结果如下所示：

![img](https://style.youkeda.com/img/course/j8/2/2-6-1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

在结果中可以看到

```
message= youkeda
```

## 接入钉钉通知

首先需要一个通知服务应用，利用kafka对接得物应用和通知应用

利用通知服务应用给钉钉系统发布消息

![](https://style.youkeda.com/img/course/j8/3/3-1-4.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

在这个设计中，我们通知服务应用和得物主体应用完全解除耦合，通过kafka消息机制进行通信。

**项目步骤**

1. 接入钉钉通知系统
2. 构建通知应用，确定消息模型
3. 通知应用接入`kafkaConsumer`
4. 改造订单服务接入`kafkaProducer`

![](https://style.youkeda.com/img/course/j8/3/3-2-1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)



![](https://style.youkeda.com/img/course/j8/3/3-2-2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)



![](https://style.youkeda.com/img/course/j8/3/3-2-3.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

![](https://style.youkeda.com/img/course/j8/3/3-2-4.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

![](https://style.youkeda.com/img/course/j8/3/3-2-5.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

> 安全设置选择自定义关键词，这个是最简单的方式，只要通知中包括关键词即可。

![](https://style.youkeda.com/img/course/j8/3/3-2-6.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

选择并拷贝Webhook地址。

### 接入钉钉

钉钉给我们提供了maven仓库

官网地址https://ding-doc.dingtalk.com/doc#/faquestions/vzbp02



首先引入Maven依赖

```xml
<dependency>
    <groupId>com.aliyun</groupId>
    <artifactId>alibaba-dingtalk-service-sdk</artifactId>
    <version>1.0.1</version>
</dependency>
```



简单发送消息代码

```java
@RequestMapping("/text")
@ResponseBody
public String sendText(){
  // #0. 配置钉钉客户端，dingdingUrl即根据刚才拷贝的Webhook
  DingTalkClient client = new DefaultDingTalkClient(this.dingdingUrl);
  // #1. request表示整个消息请求
  OapiRobotSendRequest request = new OapiRobotSendRequest();
  // #2. 请求设置消息类别
  request.setMsgtype("text");

  // #3. 设置消息内容
  OapiRobotSendRequest.Text text = new OapiRobotSendRequest.Text();
  text.setContent("得物来新订单了");
  request.setText(text);

  // #4. 设置钉钉@功能
  OapiRobotSendRequest.At at = new OapiRobotSendRequest.At();
  at.setIsAtAll(true);
  request.setAt(at);
  try {
    // #5. 发送消息请求
    client.execute(request);
  } catch (ApiException e) {
    e.printStackTrace();
  }
  return "success";
}
```

最终结果为

![](https://style.youkeda.com/img/course/j8/3/3-1-1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)



### 工程解释

注意`DingDingTest.java`是我们提供的一个钉钉通知测试类，在测试类中

```java
@Value("${dewu.notify.dingding}")
private String dingdingUrl;
```

我们利用 SpringBoot 的配置化属性配置了钉钉机器人获取到的`Webhook`。

这样就唯一标示发送到哪个钉钉群。



### 钉钉官方文档

https://ding-doc.dingtalk.com/doc#/serverapi3/iydd5h

## 构建通知应用

通知模型

1. 我们需要支持不同的通知平台

> 需要一个Enum类型表示不同的通知平台

1. 我们需要支持不同的消息类型

> 需要定义一个MsgType字段

1. 通知主体内容

> 主题、内容、图片、跳转地址等



最终模型如下

![](https://style.youkeda.com/img/course/j8/3/3-3-1.svg)



对于NotifyType，我们选择String类型，为了方便以后扩展，企业微信不一定满足text，link，markdown三个通知类型，也可能是其他关键词，所以暂时使用字符串。



通知服务设计

#### 1. 首先考虑对接平台的通知

我们初步将对接平台的接口，称为`NotifyHelper`，那么这个接口有一个发送通知的方法。

![img](https://style.youkeda.com/img/course/j8/3/3-3-2.svg)

#### 2. 完善钉钉通知和企业微信通知

紧接着我们完善钉钉通知和企业微信通知的`Helper`，uml 图如下所示：

![img](https://style.youkeda.com/img/course/j8/3/3-3-3.svg)

> 企业微信我们暂时不考虑，只是留在这儿方便以后扩展

#### 3. 接入 Kafka 消费者

我们加入接收 Kafka 消息的监听器`NotifyConsumer`，利用它完成对`Helper`的调用。

![img](https://style.youkeda.com/img/course/j8/3/3-3-4.svg)



首先完成`DingDingHelper`类，完善其中的 4 个方法

```java
// 根据不同的Notify的NotifyType调用不同的方法
public boolean sendNotify(Notify notify);

// 以下三个方法，将之前DingDingTest中写死的参数，替换成Notify中的属性
public boolean sendText(Notify notify);
public boolean sendLink(Notify notify);
public boolean sendMarkdown(Notify notify);
```

加入`Kafka`的配置，并且修改`NotifyConsumer`，将其当做通知消息的消费者。

注意 Kafka 配置如下：

```java
customerGroupId: "dewuNotify"
topic: "notify"
```

再次提醒大家，需要执行如下步骤

1. `pom.xml`文件中添加对应的依赖
2. `application.properties`中添加 Kafka 消费者配置信息
3. 完善`NotifyConsumer`接入 Kafka 消费者

然后`NotifyConsumer`，主要完成如下两步：

1. `NotifyConsumer` 完成对 Kafka 消息的反序列化
2. 根据平台调用不同的`Helper`

#### 如何进行反序列化呢？

> 可以复习一下 Java 网络编程中的序列化和反序列化

## 关于序列化和反序列化

**实现序列化的必备要求：**

​    只有实现了Serializable或者Externalizable接口的类的对象才能被序列化为字节序列。（不是则会抛出异常）

Fastjson 应用

### 添加 maven 依赖

```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>x.x.x</version>
</dependency>
```


### Fastjson API
### 定义 Bean
#### Group.java

```
public class Group {
    private Long       id;
    private String     name;
    private List<User> users = new ArrayList<User>();
}
```



#### User.java

```
public class User {
    private Long   id;
    private String name;
}
```

#### 初始化 Bean

```
Group group = new Group();
group.setId(0L);
group.setName("admin");

User guestUser = new User();
guestUser.setId(2L);
guestUser.setName("guest");

User rootUser = new User();
rootUser.setId(3L);
rootUser.setName("root");

group.addUser(guestUser);
group.addUser(rootUser);
```



#### 序列化
`String jsonString = JSON.toJSONString(group);`
`System.out.println(jsonString);`

#### 反序列化

`Group bean = JSON.parseObject(jsonString, Group.class);`

## 数据流

`Kafka Streams`到底是个**什么东西，用在什么场景，处理什么问题**？ （本章节我们都偏向理论，当然理解了理论才能更好的实践）

总结成一句话： **它是用来支持流式处理**。那到底什么是流式处理呢？

我相信大家看到**流**的第一反应 --- **水流**。

![img](https://style.youkeda.com/img/course/j8/4/1-1.jpeg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

如果脑袋里浮出这样的景象，很赞喔，我们简单将其当做对类似水流形式的处理。

河里面流动的是水，而应用里面流动的是什么呢？当然是数据，我们也可以称之为**数据流**。

### 数据流

数据流也被称为事件流，试想一下，对于淘宝网站，哪些东西是数据流？

1. 我们在浏览网页时候的浏览数据
2. 在搜索商品的搜索数据
3. 购买商品数据
4. etc...

对于淘宝而言，这些数据是无处不在，源源不断的，这就是**数据流**。

再抽象一层，数据流有什么特点呢？

**1. 无穷的，或者说无边界的**

我们之前接触的数据，大多数是有限的，比如某天的访问量，某个季度的数据

> 无界的表示数据是无限增长的

**2. 无处不在的**

从上面分析可以知道，网站上发生的一切，都是数据流

**3. 有序的**

数据的到来总有个先后顺序

**4. 不可变的**

数据一旦产生，就不能被改变，数据流表示的是某一时刻的事实，时间是无法倒流的

**5. 可重播**

既然数据是无法改变的，在不改变数据的情况下，结果肯定是固定的

也就是如果将数据重新跑 N 次，结果总是相同的，



















