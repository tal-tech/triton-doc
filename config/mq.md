### 队列配置
------
本节主要介绍triton的队列配置，主要包含四种消息队列，分别是kafka、rabbitmq、rocketmq、nsq，配置文件格式为json
```json
{
  "enabled": {
    "kafka": true,
    "rabbitmq": false,
    "rocketmq": false,
    "nsq": false
  },
  "kafka": [
    {
      "consumerGroup": "test0",
      "consumerCount": 1,
      "host": [
        "127.0.0.1:9092"
      ],
      "sasl": {
        "enabled": false,
        "user": "",
        "password": ""
      },
      "topic": "test",
      "failTopic": "xes_exercise_fail",
      "tplMode": 1,
      "batchSize": 5,
      "batchTimeout": 10000,
      "tplName": ""
    }
  ],
  "nsq": [
    {
      "consumerCount": 2,
      "nsqLookup": [
        "127.0.0.1:4161"
      ],
      "topic": "test",
      "channel": "nsq",
      "tplMode": 0
    }
  ],
  "rocketmq": [
    {
      "consumerGroup": "test0",
      "consumerMode": 0,
      "nameServer": [
        "127.0.0.1:9876"
      ],
      "topic": "TopicTest",
      "tags": "stu_test_0",
      "offset": 0,
      "retry": 2,
      "consumerCount": 1,
      "tplMode": 1
    }
  ],
  "rabbitmq": [
    {
      "url": "amqp://guest:guest@localhost:5672/",
      "consumerQueue": "test",
      "failExchange": "failExchange",
      "failCount": 3,
      "isReject": true,
      "prefetchCount": 10,
      "consumerCount": 2,
      "tplMode": 1
    }
  ]
}
```

* enabled控制消息队列的入口
* kafka，是一个数组，可以支持多个不同topic消费
    * consumerGroup：消费组
    * consumerCount：消费者数量
    * host：broker地址
    * sasl：kafka安全认证
    * topic：消费的topic
    * failTopic：失败消息队列
    * tplMode：两种模式
        * 1：消费topic
        * 2：消费某一个topic下某个tag（默认消息体按空格切分，第一个元素为tag）
    * tplName:消费逻辑name，若为空，默认为topic名称
    * batchSize：批量消费size，到达数量，调用处理方法
    * batchTimeout：批量消费等待超时（ms），超过时间，但未到达batchSize，同样触发处理方法
* rabbitmq，是一个数组，可以支持消费不同的queue
    * url：rabbitmq地址
    * consumerQueue：消费队列
    * failExchange：处理失败的消息要回传的exchange
    * failCount：若处理失败，重试次数
    * isReject：failExchange为空时生效，表示是否回放到原queue
    * prefetchCount：消费者预先消费消息数
    * consumerCount：消费者数量
    * tplMode：同kafka
* rocketmq，是一个数组，可以支持消费不同的topic
    * consumerGroup：消费组
    * consumerMode：消费模式（rocketmq支持不同的消费模式，目前triton只支持PushConsumer，请固定配置为0）
    * nameServer：nameserver地址
    * topic：消费Topic
    * tags：rocketmq原生支持的相同topic不同tag
    * retry：消费失败重试次数
    * consumerCount：消费者数量
    * tplMode：同kafka
* nsq，是一个数组，可以支持消费不同topic
    * consumerCount：消费数量
    * nsqLookup：nsqLookup地址
    * topic：消费的topic
    * channel：消费的channel名称
    * tplMode：同kafka
