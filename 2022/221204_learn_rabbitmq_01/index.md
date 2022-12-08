# 学习rabbitmq(一)：准备工作

## 前言
Rabbitmq是使用Erlang语言实现AMQP协议的消息中间件，具有易用、高扩展、高可用、持久化等方面特点。

## 环境准备

### docker安装
选择docker compose方式安装，新建docker-compose.yml文件

```yaml
version: "3"
services:
  rabbitmq:
    image: rabbitmq:management
    container_name: rabbitmq
    restart: always
    ports:
      - "5672:5672"
      - "15672:15672"
    environment:
      RABBITMQ_DEFAULT_USER: "guest"
      RABBITMQ_DEFAULT_PASS: "guest"
```
```shell 
docker compose up -d
```

## 名词解释
### 组件介绍

- Broker：一个RabbitMQ实例就是一个Broker
- Connection：Publisher/Consumer与Broker之间的TCP连接。
- Virtual Host：虚拟主机，用于逻辑隔离，一个虚拟主机里面可以有若干个不同名的Exchange和Queue。每个vhost都拥有自己的队列、交换机、绑定和权限机制。vhost必须在连接时指定，默认的vhost是"/"。
- Routing Key：路由键，Publisher将消息发送给Exchange的时候，会发送一个RoutingKey，用来指定路由规则，这样交换器就知道把消息发送到哪个队列。路由键通常为一个"."分割的字符串，例如"com.rabbitmq"。
- Exchange：交换机，接收Publisher发送的消息按照路由规则路由到一个或多个队列，常用的交换机类型共有以下四种
  - direct：将消息发给Exchange中指定Routing Key的Queue中。
  - fanout：不需要指定Routing Key，直接将消息发送给Exchange中的所有Queue。
  - topic：匹配模式，消息发送到匹配规则的Routing key的Queue中，有"\*"与"#"两个通配符，"\*"表示只匹配一个词，"#"表示匹配多个。比如："com.rabbitmq"、"com.rabbitmq.exchange"，"com.*"只能匹配到"com.rabbitmq"，"com.#"两个都能匹配到
  - headers：根据消息体的headers匹配，绑定的时候指定相关header参数即可。
- Message：消息，它是由消息头和消息体组成。消息头则包括Routing-Key、Priority（优先级）等。
- Queue：消息队列，用来保存消息，供消费者消费。一个消息可投入一个或多个队列。
- Banding：绑定关系，Exchange和Queue之间的虚拟连接。通过Routing Key将Exchange和Queue关联起来。
- Channel：管道，一条双向数据流通道。发布消息、接收消息、订阅队列，都是通过管道完成。
- Publisher：消息的生产者。
- Consumer：消息的消费者。

### 参数说明
- Routing Key：Queue绑定参数，用来指定路由规则，Exchange根据路由键把消息发送到对应的Queue。
- Type：Exchange的参数，有四种类型
- Durable：Exchange和Queue都有的参数，参数类型为boolean，表示是否持久化
- Auto delete：Exchange和Queue都有的参数，参数类型为boolean
  - Exchange中表示当所有绑定队列都不在使用时，是否自动删除交换机，true为删除，false为不删除
  - Queue中表示当所有消费客户端连接断开后，是否自动删除队列，true为删除，false为不删除
- Internal：Exchange的参数，参数类型为boolean，表示不能对这个Exchange发送消息，但可以通过管理后台发送消息
- noWait：Exchange、Queue、QueueBind都有的参数，类型为boolean，表示无需等待，操作请求发出去不用服务端的返回值，直接进行下一步
- Exclusive：Queue的参数，类型为boolean，表示为私有、独占队列，只对创建该队列的用户可见，其它用户无法访问
- Mandatory：Publish时候的参数，类型为boolean，表示告诉服务器，如果消息不可路由，应该将消息返回给发送者，并通知失败，且仅通知失败。
- ~~Immediate：Publish时候的参数，类型为boolean，表示告诉服务器，如果该消息关联的Queue上有消费者，则马上将消息投递给它，如果所有Queue都没有消费者，直接把消息返还给生产者，不用将消息入队列等待消费者（rabbitmq 3.0之后删除了该参数）~~

## 五种消息模型
- 普通模型
- 工作模型
- 订阅模型(发布订阅之fanout)
- 路由键模型(发布订阅之direct)
- 主题模型(发布订阅之topic)

---

> 作者: [Jay](https://github.com/Heelie)  
> URL: https://heelie.github.io/2022/221204_learn_rabbitmq_01/  

