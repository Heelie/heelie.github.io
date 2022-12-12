# 学习rabbitmq(二)：普通模型

## 前言

代码实现为go语言版本，rabbitmq的guest用户只能本地登陆，远程需要单独创建用户

## 项目初始化

首先初始化一个go mod项目
```shell
mkdir rabbitmq_study
cd rabbitmq_study
go mod init rabbitmq_study
```

安装RabbitMQ的Go语言客户端库：
```shell
go get github.com/rabbitmq/amqp091-go
```

## 代码实现

### 连接RabbitMQ服务器
```go
conn, err := amqp.Dial("amqp://guest:guest@localhost:5672/")
if err != nil {
    log.Fatal(err)
}
defer conn.Close()
```

### 创建一个通道
```go
ch, err := conn.Channel()
if err != nil {
    log.Fatal(err)
}
defer ch.Close()
```

### 声明一个持久化的队列
```go
queue, err := ch.QueueDeclare(
    "simple_queue", // 队列名
    true,           // 是否持久化
    false,          // 是否自动删除
    false,          // 是否独占，排它
    false,          // 是否不等待，丢弃结果
    nil,            // 其他参数
)
if err != nil {
    log.Fatal(err)
}
```

### 发送消息

> 普通消息模型的生产者和消费者为点对点模式，两者交互只需要一个消息队列，不需要交换机和路由键系列的绑定

```go
err = ch.PublishWithContext(ctx,
    "",         // 交换机名，普通模式没有交换机
    queue.Name, // 路由键名，普通模式没有路由键，此处为队列名
    false,      // 是否强制，失败需要通知
    false,      // 是否立即发送给能够接收消息的消费者
    amqp.Publishing{
        ContentType: "text/plain",
        Body:        []byte("Hello World!"),
    })
if err != nil {
    log.Fatal(err)
}
```

### 获取一个消息接收器
```go
msgs, err := ch.Consume(
    "simple_queue", // 队列名
    "",             // 消费者标识
    true,           // 自动应答
    false,          // 是否独占，排它
    false,          // 是否只消费远程消息
    false,          // 是否不等待，丢弃结果
    nil,            // 其他参数
)
if err != nil {
    log.Fatal(err)
}
```

### 不断读取消息
```go
for msg := range msgs {
    // 处理消息
    fmt.Println(string(msg.Body))
	// 如果消息接收器不是自动应答，就需要手动ack，第二个参数表示是否批量应答
    // ch.Ack(msg.DeliveryTag, false) 
}
```

## 完整代码
```go
package main

import (
	"context"
	"fmt"
	amqp "github.com/rabbitmq/amqp091-go"
	"log"
	"time"
)

func init() {
	log.SetFlags(log.Flags() | log.Llongfile)
}

func main() {
	// 连接RabbitMQ服务器
	conn, err := amqp.Dial("amqp://guest:guest@localhost:5672/")
	if err != nil {
		log.Fatal(err)
	}
	defer conn.Close()

	// 创建一个通道
	ch, err := conn.Channel()
	if err != nil {
		log.Fatal(err)
	}
	defer ch.Close()

	// 声明一个持久化的队列
	queue, err := ch.QueueDeclare(
		"simple_queue", // 队列名
		true,           // 是否持久化
		false,          // 是否自动删除
		false,          // 是否独占，排它
		false,          // 是否不等待，丢弃结果
		nil,            // 其他参数
	)
	if err != nil {
		log.Fatal(err)
	}

	ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
	defer cancel()

	// 发送消息
	err = ch.PublishWithContext(ctx,
		"",         // 交换机名，普通模式没有交换机
		queue.Name, // 路由键名，普通模式没有路由键，此处为队列名
		false,      // 是否强制，失败需要通知
		false,      // 是否立即发送给能够接收消息的消费者
		amqp.Publishing{
			ContentType: "text/plain",
			Body:        []byte("Hello World!"),
		})
	if err != nil {
		log.Fatal(err)
	}

	// 获取一个消息接收器
	msgs, err := ch.Consume(
		"simple_queue", // 队列名
		"",             // 消费者标识
		true,           // 自动应答
		false,          // 是否独占，排它
		false,          // 是否只消费远程消息
		false,          // 是否不等待，丢弃结果
		nil,            // 其他参数
	)
	if err != nil {
		log.Fatal(err)
	}

	// 不断读取消息
	for msg := range msgs {
		// 处理消息
		fmt.Println(string(msg.Body))
        // 如果消息接收器不是自动应答，就需要手动ack，第二个参数表示是否批量应答
        // ch.Ack(msg.DeliveryTag, false) 
	}
}
```

---

> 作者: [Jay](https://github.com/Heelie)  
> URL: https://heelie.github.io/2022/221207_learn_rabbitmq_02/  

