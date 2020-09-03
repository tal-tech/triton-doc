### print模板
-----
print主要的功能就是打印，是模板中最简单的，我们可以参照[tpl/init.go](https://github.com/tal-tech/triton/blob/master/tpl/init.go)的代码实现
```go
func init() {
	funcsMap["printf"] = func(format string, a ...interface{}) (n int, err error) {
		return fmt.Printf(format, string(a[0].([]byte)))
	}
}
```
printf接收两个参数，一个是format，另一个就是数据，要使用printf，我们要在模板中这样配置
```shell
[test]
-={{$ctx := .Ctx}}{{$arg := .Data}}{{printf "%v\n" $arg}}
-=@NONE
```