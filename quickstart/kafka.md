### kafka
-----

本小节主要介绍如何使用triton快速完成kafka消息的消费，在此不再对kafka做相关的介绍，不太了解kafka的可以去[kafka官网](https://kafka.apache.org/)阅读相关文档。

#### 1、启动zookeeper
```shell
./bin/zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties
```
#### 2、启动kafka
```shell
./bin/kafka-server-start /usr/local/etc/kafka/server.properties
```
#### 3、创建topic
```shell
./bin/kafka-topics  --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test
```
#### 4、修改kafka相关配置
* 基础配置请参照[基础配置](../config/base.md)，基本上不需要做什么改动
* 队列配置如下
```json
{
  "enabled": {
    "kafka": true
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
      "tplMode": 1
    }
  ]
}
```
* 模板配置如下
```shell
[test]
-={{$ctx := .Ctx}}{{$arg := .Data}}{{printf "%v\n" $arg}}
-=@NONE
```
这个模板其实对应的是[tpl/init.go](https://github.com/tal-tech/triton/blob/master/tpl/init.go)中funcMap中的printf逻辑，将消费到的消息进行打印。
>* 如果tplMode为1，[test]指的就是消费topic为test中的消息，然后执行printf方法
>* 如果tplMode为0，[test]指的就是消费topic为test中tag为test的消息，然后执行printf方法

#### 5、编译
```shell
make
```
#### 6、运行
```shell
./bin/triton -c ../conf/conf.ini
```