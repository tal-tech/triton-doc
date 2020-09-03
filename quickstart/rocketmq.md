### rocketmq
-----

本小节主要介绍如何使用triton快速完成rocketmq消息的生产，在此不再对rocketmq做相关的介绍，不太了解rocketmq的可以去[rocketmq官网](https://rocketmq.apache.org/)阅读相关文档。

#### 1、启动NameServer
```shell
sh mqnamesrv
```
#### 2、启动Broker
```shell
sh mqbroker -n localhost:9876 autoCreateTopicEnable=true
```
#### 3、修改rocketmq相关配置
* 基础配置请参照[基础配置](../config/base.md)，基本上不需要做什么改动
* 队列配置如下
```json
{
  "enabled": {
    "rocketmq": true
  },
  "rocketmq": [
      {
        "consumerGroup": "test0",
        "consumerMode": 0,
        "nameServer": [
          "127.0.0.1:9876"
        ],
        "topic": "TopicTest",
        "tags": "stu_test_0",
        "retry": 2,
        "consumerCount": 1,
        "tplMode": 1
      }
  ]
}
```
* 模板配置如下
```shell
[stu_test_0]
-={{$ctx := .Ctx}}{{$arg := .Data}}{{printf "%v\n" $arg}}
-=@NONE
```
这个模板其实对应的是[tpl/init.go](https://github.com/tal-tech/triton/blob/master/tpl/init.go)中funcMap中的printf逻辑，将消费到的消息进行打印。
>* 如果tplMode为1，[stu_test_0]指的就是消费topic为TopicTest，tag为stu_test_0的消息，然后执行printf方法
>* tplMode为0，我们不建议在rocketmq中这么使用，tplMode是为了kafka不支持相同topic不同tag而设置的
#### 4、编译
```shell
make
```
#### 5、运行
```shell
./bin/triton -c ../conf/conf.ini
```