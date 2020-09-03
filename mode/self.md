### 自定义模板
-----
除了上述几种类型的模板以外，有些时候我们对消费的消息可能要进行复杂的处理，那么使用golang的模板语言进行业务逻辑的编排就非常困难，所以我们提供的自定义模板类型；我们可以自定义方法名称，方法用golang的代码进行实现，只需在模板配置中调用方法名称即可，例如我们定义一个self方法，代码实现如下：
```go
funcsMap["self"] = func(v interface{}) (ret string, err error) {
		vStr := cast.ToString(v)
		if strings.Contains(vStr, "kafka"){
			fmt.Println("kafka exist")
		}else {
			fmt.Println("kafka not exist")
		}
		return "OK", nil
	}
```
模板配置如下
```shell
[test]
-={{$ctx := .Ctx}}{{$arg := .Data}}{{self $arg}}
-=@NONE
```

我们调用kafka的producer客户端，向名为test的topic发送数据
```shell
/usr/local/Cellar/kafka/2.3.1 » bin/kafka-console-producer --broker-list localhost:9092 --topic test                                                        hehaitao@localhost
>kafka1
>rabbitmq
```

我们可以看到triton的输出
```shell
/project/tal/src/git.100tal.com/wangxiao_go_center/triton(master*) »   ./bin/triton -c ../conf/conf.ini                                                    hehaitao@localhost
2019/12/10 17:01:43 CONF INIT,path:../conf/conf.ini
[2019/12/10 17:01:43 CST] [INFO] (pprofutil.go:22:1840450000000086	localhost) Pprof	open pprof on port:9113
2019/12/10 17:01:43 Conf,err:section 'MysqlCluster' not found
[2019/12/10 17:01:43 CST] [INFO] (tpl.go:37:1840450000000001	localhost) TplInit	TplInitSuccess
[2019/12/10 17:01:43 CST] [INFO] (kafka.go:116:1840450000000115	localhost) Consume	Start consume from broker [127.0.0.1:9092]
kafka exist
[2019/12/10 17:01:54 CST] [INFO] (ctrl.go:81:1840450000000115	localhost) deal	ret:true,key:,value:test kafka1
kafka not exist
[2019/12/10 17:03:50 CST] [INFO] (ctrl.go:81:1840450000000115	localhost) deal	ret:true,key:,value:test rabbitmq
```
这样我们就实现了一个自定义的模板，这里的示例很简单，你可以根据自己的需求来实现复杂的业务逻辑。

#### 注意
若处理结果返回error，并且结果非"OK",处理逻辑才被认为是失败的。