
---
title: RabbitMQ-quickStart
date: 2022-02-27 10:13:00
author: 丁兆龙
img: http://r7lh60uut.hb-bkt.clouddn.com/RabbitMQ/RabbitMQ.jpg
top: false
mathjax: false
summary: RabbitMQ快速开始示例
categories: 
  - quickStart

tags:
  - 后端
  - 消息队列
  - RabbitMQ

---

## Maven依赖

```xml
<-- SpringBoot整合RabbitMQ的起步依赖 -->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-amqp</artifactId>
</dependency>

<-- RabbitMQ集成测试 -->    
<dependency>
	<groupId>org.springframework.amqp</groupId>
	<artifactId>spring-rabbit-test</artifactId>
	<scope>test</scope>
</dependency>
```



## SpringBoot配置

```yml
server:
  port: 8080
spring:
  application:
    # 项目名字
    name: rabbitmq-demo
  # 配置RabbitMQ服务器:  主机,端口号,用户名和密码等
  rabbitmq:
    host: 127.0.0.1
    port: 5672
    username: guest
    password: guest
```



## RabbitMQ主配置类

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * 启动类上需要加入@EnableRabbit注解,表示开启基于注解的RabbitMQ模式.
 */
@EnableRabbitMQ
@SpringBootApplication
public class RabbitMQDemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(MsgSenderApplication.class, args);
    }

}
```



> RabbitMQ的交换机(Exchange),队列(Queue)以及绑定(Binding)的配置:
>
> **如果仅作为消息消费者,则不需要此配置; 如果添加了此配置,则也具备发送消息的功能.**

```java
import org.springframework.amqp.core.Binding;
import org.springframework.amqp.core.BindingBuilder;
import org.springframework.amqp.core.DirectExchange;
import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;


/**
 * RabbitMQ的配置类
 */
@Configuration
public class DirectRabbitConfig {

    /**
     * 创建消息队列
     * 名称 directQueue
     */
    @Bean
    public Queue directQueue() {
        // durable:是否持久化,默认是false,持久化队列：会被存储在磁盘上，当消息代理重启时仍然存在，暂存队列：当前连接有效
        // exclusive:默认也是false，只能被当前创建的连接使用，而且当连接关闭后队列即被删除。此参考优先级高于durable
        // autoDelete:是否自动删除，当没有生产者或者消费者使用此队列，该队列会自动删除。

        //一般设置一下队列的持久化就好,其余两个就是默认false
        return new Queue("directQueue", true);
    }


    /**
     * 创建交换机, 此交换机是直连交换机(即Direct-Exchange)
     * 名称 directExchange
     */
    @Bean
    DirectExchange directExchange() {
        return new DirectExchange("directExchange", true, false);
    }

    /**
     * 创建绑定, 将消息队列和交换机绑定, 并设置路由键(即Routing-Key)
     */
    @Bean
    Binding bindingDirect() {
        return BindingBuilder.bind(directQueue()).to(directExchange()).with("directRouting");
    }

}
```





> **消息发送方**的消息推送示例:

```java
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;
import java.time.LocalDateTime;
import java.util.HashMap;
import java.util.Map;
import java.util.UUID;

@RestController
public class SendMessageController {

    /**
     * 使用RabbitTemplate, 提供了接收/发送等等方法
     */
    @Resource
    RabbitTemplate rabbitTemplate;

    @GetMapping("/sendDirectMessage")
    public String sendDirectMessage(String msg) {

        String messageId = String.valueOf(UUID.randomUUID());
        String createTime = LocalDateTime.now().toString();
        Map<String, Object> map = new HashMap<>(8);
        map.put("messageId", messageId);
        map.put("messageData", msg);
        map.put("createTime", createTime);
        //将消息携带绑定键值：TestDirectRouting 发送到交换机TestDirectExchange
        rabbitTemplate.convertAndSend("directExchange", "directRouting", map);
        return "ok";
    }

}
```



> **消息接收方**消费消息:

```java
import org.springframework.amqp.rabbit.annotation.*;
import org.springframework.stereotype.Component;

import java.util.Map;

/**
 * 创建RabbitMQ的队列监听
 */
@Component
@RabbitListener(queues = "directQueue")
public class DirectReceiver {

    /**
     * @param testMessage 消息队列中的数据, 数据类型与发送的类型相对应
     */
    @RabbitHandler
    public void process(Map<String, Object> testMessage) {
        System.out.println("消息队列的接收方收到消息: " + testMessage.toString());
    }

}
```





## 预期结果



> 需要首先开启RabbitMQ服务器并正确配置连接.
>
> RabbitMQ管理界面地址:  [RabbitMQ Management](http://localhost:15672/#/)

1. 初始状态: 消息队列无消息, 无新建的交换机:point_down:

![](http://r7lh60uut.hb-bkt.clouddn.com/RabbitMQ/RabbitMQ%E7%AE%A1%E7%90%86%E7%95%8C%E9%9D%A2.png)



2. 消息发送端将消息队列绑定交换机, 并将消息推送到消息队列中:point_down:

![](http://r7lh60uut.hb-bkt.clouddn.com/RabbitMQ/RabbitMQ%E6%B6%88%E6%81%AF%E5%8F%91%E9%80%81%E7%AB%AF.png)



3. 目前的消息队列中暂存了100条消息,这些消息尚为被消费:point_down:



![](http://r7lh60uut.hb-bkt.clouddn.com/RabbitMQ/RabbitMQ%E7%AE%A1%E7%90%86%E7%95%8C%E9%9D%A22.png)



4. 开启已经监听指定消息队列的消息接收端, 消费队列中的消息:point_down:

![](http://r7lh60uut.hb-bkt.clouddn.com/RabbitMQ/RabbitMQ%E6%B6%88%E6%81%AF%E6%B6%88%E8%B4%B9%E7%AB%AF.png)



5. 消息推送到消息队列暂存-->开启配置队列监听的消息消费端-->消费消息队列中的消息 已完成, 消息队列已被清空:point_down:

![](http://r7lh60uut.hb-bkt.clouddn.com/RabbitMQ/RabbitMQ%E7%AE%A1%E7%90%86%E7%95%8C%E9%9D%A23.png)

