### 基础配置
------
本节主要介绍triton的基础配置，与我们提供的其他框架类似，主要是一些进程监控和日志配置。
```shell
[DEFAULT]
tpl=../conf/curl.ini
mqConfigPath=/home/www/triton/conf/config.json

[Ignore]
name=../conf/ignore.ini

[PerfProxy]
level=I
timedebug=false
host=127.0.0.1:13333

[Pprof]
enable=true
port=9113

[log]
LogPath=/home/log/xeslog/triton/triton.log
Level=INFO
RotateSize=1G
RotateDaily=true

```

* Pprof是golang进程堆栈监控
* PerfProxy是采集打点数据
* log是日志配置
* Ignore中是需要忽略的模板配置(必需)
* DEFAULT是一些默认的配置，包含两项
    * tpl是[模板配置文件](mode.md)(必需)
    * mqConfigPath是[队列配置文件](mq.md)(必需)