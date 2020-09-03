### 模板配置
------
本节主要介绍triton的模板配置，我们提供的是go
```shell
[xesv5]
-={{$arg := .}}{{echo $arg}}
-=@NONE

[topic]
-={{$arg := .}}{{echo $arg}}
-=@NONE

[rabbitmq]
-={{$ctx := .Ctx}}{{$arg := .Data}}{{printf "%v\n" $arg}}
-=@NONE

[demoSo]
-={{$arg := .Data}}{{$body := printf  "%s" $arg}}
-=@SO
-=testplugin.so
-={{$body}}
-=param2

[demoCurl]
-={{$arg := .Data}}{{$body := printf  "%s" $arg}}
-=@CURL
-=POST -H 'Content-Type: application/x-www-form-urlencoded' http://127.0.0.1:9898/demo/test
-=param={{$body}}
-=@RET
-=REG:"code":0
-=@END

[demoSql]
-={{$arg := .Data}}{{$body := printf  "%s" $arg}}
-=@SQL
-=USE:instance
-=select * from dbtest;
-=update dbtest set param3="{{$body}}" where id=1;


[autosort]
-={{$f := split "\t" .}}{{$o := index $f "_0"}}
-=@CURL
-=PUT http://xxx?val={{`autosort-`}}{{$o}}
-=@END
[FlashSaleRemindCron]
-={{$f := split "\t" .}}{{$param := index $f "_0"}}
-={{$body := printf "{\"result\": {\"action\": \"TimersCallback\", \"model\": \"FlashSaleRemind\", \"parameters\": {%s}}}" $param }}
-={{$sign := md5 $body "xxx"}}
-=@CURL
-=POST -H "Content-Type: application/json" -H "MIJIA-ENV: staging" http://xxxxx/xxxx
-={{$body}}
-=@RET
-=REG:Success
-=@END
-=select 1;
```

以上是我们给出的模板配置示例，具体的模板实现可以参照[模板](../mode/mode.md)