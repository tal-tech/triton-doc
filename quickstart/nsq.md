### nsq
-----

本小节主要介绍如何使用triton快速完成nsq消息的消费，在此不再对nsq做相关的介绍，不了解nsq的可以去[nsq官网](https://nsq.io/)阅读相关文档。

#### 1、启动nsqlookupd
```shell
./bin/nsqlookupd
```
#### 2、启动nsqd
```shell
./bin/nsqd --lookupd-tcp-address=127.0.0.1:4160
```
#### 3、修改nsq相关配置
* 基础配置请参照[基础配置](../config/base.md)，基本上不需要做什么改动
* 队列配置如下
```json
{
  "enabled": {
    "nsq": true
  },
  "nsq": [
      {
        "consumerCount": 2,
        "nsqLookup": [
          "127.0.0.1:4161"
        ],
        "topic": "test",
        "channel": "nsq",
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

#### 4、编译
```shell
make
```
#### 5、运行
```shell
./bin/triton -c ../conf/conf.ini
```