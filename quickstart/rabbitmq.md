### rabbitmq
-----

本小节主要介绍如何使用triton快速完成rabbitmq消息的消费，在此不再对rabbitmq做相关的介绍，不太了解rabbitmq的可以去[rabbitmq官网](https://www.rabbitmq.com/)阅读相关文档。

#### 1、启动Rabbitmq
```shell
./rabbitmq-server
```
#### 2、修改rabbitmq相关配置
* 基础配置请参照[基础配置](../config/base.md)，基本上不需要做什么改动
* 队列配置如下
```json
{
  "enabled": {
    "rabbitmq": true
  },
  "rabbitmq": [
      {
        "url": "amqp://guest:guest@localhost:5672/",
        "consumerQueue": "test",
        "failExchange": "failExchange",
        "failCount": 3,
        "isReject": false,
        "prefetchCount": 10,
        "consumerCount": 2,
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
>* 如果tplMode为1，[test]指的就是消费名为test的queue中的消息，然后执行printf方法
>* 如果tplMode为0，[test]指的就是消费名为test中tag为test的消息，然后执行printf方法(我们不建议在rabbitmq中这么使用，tplMode是为了kafka不支持相同topic不同tag而设置的)
#### 3、编译
```shell
make
```
#### 4、运行
```shell
./bin/triton -c ../conf/conf.ini
```