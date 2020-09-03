### curl模板
-----
curl主要的功能就是发送http请求，也是我们常用的功能，我们提供了一个模板示例，如下
```shell
[demoCurl]
-={{$arg := .Data}}{{$body := printf  "%s" $arg}}
-=@CURL
-=POST -H 'Content-Type: application/x-www-form-urlencoded' http://127.0.0.1:9898/demo/test
-=param={{$body}}
-=@RET
-=REG:"code":0
-=@END
```