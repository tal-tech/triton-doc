### sql模板
-----
sql主要的功能就是执行sql语句，也是我们常用的功能，我们提供了一个模板示例，如下
```shell
[demoSql]
-={{$arg := .Data}}{{$body := printf  "%s" $arg}}
-=@SQL
-=USE:instance
-=select * from dbtest;
-=update dbtest set param3="{{$body}}" where id=1;
```