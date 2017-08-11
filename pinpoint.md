# Pinpint性能监控利器

## 官方主页：https://github.com/naver/pinpoint

## Pinpoint是一个分析大型分布式系统的平台，提供解决方案来处理海量跟踪数据。2012年七月开始开发，2015年1月9日作为开源项目启动。
- pinpoint主要有四个部分组成：

 - agent：其中包含一些插件，用于客户端向收集器发送跟踪数据。

 - pinpoint-collector：收集器，处理agent端发送过来的数据，并持久化。

 - pinpoint-web：展示统计数据。

 - hbase：持久化层。

## hbase初始化

- 下载包：<http://mirror.bit.edu.cn/apache/hbase/1.1.11/hbase-1.1.11-bin.tar.gz>
- 安装手册：<http://abloz.com/hbase/book.html#quickstart>

- install hbase

 - 指定JDK路径:
 - 
`vi hbase-config.sh +27
JAVA_HOME="/data/packages/jdk1.8.0_60"
./start-hbase.sh
`
    - hbase默认TcpPort:16010 


## import tables

- hbase-create.hbase #从官网获取

 - `/data/packages/hbase-1.1.11/bin`

 - `./hbase shell`

 - 复制SQL执行完
 - 登录: <http://172.16.51.7:16010/master-status>

## Hbase表优化
- 修改hbase表的存储时间，建表的默认TTL时间比较长，需要比较大的磁盘空间，我们可以修改一下 默认的TTL时间

- 修改的脚本地址为：alterTable7Days.hbase，然后复制脚本内容，粘贴执行。执行完之后可以查看表信息，`describe 'AgentInfo'`

- 可以看到 TTL => '604800 SECONDS (7 DAYS)。


## pinpoint

- `https://github.com/naver/pinpoint/blob/1.6.x/doc/installation.md`
- `https://github.com/naver/pinpoint/releases/tag/1.6.2`
- `http://www.cnblogs.com/yyhh/p/6106472.html`


- pinpoint-collector-1.6.2

 - 下载TOMCAT包，解压pinpoint-collector.war
 - `unzip pinpoint-collector-1.6.2.war -d ROOT`

- pinpoint-web-1.6.2

 - 编辑hbase配置文件: `vi /data/web/tomcat8-pinpoint-web/webapps/ROOT/WEB-INF/classes/hbase.properties`

``` bash
  hbase.client.host=localhost
  hbase.client.port=2181
```

- pinpoint agent
 - 编辑配置文件: `vi /data/pinpoint-agent/pinpoint.config`
 - `profiler.collector.ip=172.16.51.7`

- 添加tomcat服务启动脚本catalina.sh
``` bash
CATALINA_OPTS="$CATALINA_OPTS -javaagent:/data/pinpoint-agent/pinpoint-bootstrap-1.6.2.jar"
CATALINA_OPTS="$CATALINA_OPTS -Dpinpoint.agentId=jdapp20170810"
CATALINA_OPTS="$CATALINA_OPTS -Dpinpoint.applicationName=jdapp
```

- 查看pinpoint进程 `ps -ef |grep pinpoint`
``` bash 
-javaagent:/data/pinpoint-agent/pinpoint-bootstrap-1.6.2.jar 
-Dpinpoint.agentId=jdapp20170810 
-Dpinpoint.applicationName=jdapp
```
