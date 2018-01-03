Flume1.6安装（Centos6.8），Flume采集Nginx日志上传至HDFS中
一、下载Flume1.6
wget http://archive.apache.org/dist/flume/1.6.0/apache-flume-1.6.0-bin.tar.gz

二、解压，移至/usr/local目录下
tar -zxvf apache-flume-1.6.0-bin.tar.gz
mv apache-flume-1.6.0-bin flume
mv flume/ /usr/local

三、修改环境变量
export FLUME_HOME=/usr/local/flume
export PATH=$PATH:/sbin:/bin:/usr/sbin:/usr/bin:/usr/X11R6/bin:$JAVA_HOME/bin:$FLUME_HOME/bin

四、使环境变量生效
source /etc/profile

五、测试安装情况
flume-ng version

六、修改flume-conf.properties文件
cp flume-conf.properties.template flume-conf.properties
# 配置Agent
a1.sources = r1
a1.sinks = k1
a1.channels = c1

# 配置Source
a1.sources.r1.type = exec
a1.sources.r1.channels = c1
a1.sources.r1.deserializer.outputCharset = UTF-8

# 配置需要监控的日志输出目录
a1.sources.r1.command = tail -F /usr/local/nginx/log/access.log

# 配置Sink
a1.sinks.k1.type = hdfs
a1.sinks.k1.channel = c1
a1.sinks.k1.hdfs.useLocalTimeStamp = true
a1.sinks.k1.hdfs.path = hdfs://master:9000/flume/events/%Y-%m
a1.sinks.k1.hdfs.filePrefix = %Y-%m-%d-%H
a1.sinks.k1.hdfs.fileSuffix = .log
a1.sinks.k1.hdfs.minBlockReplicas = 1
a1.sinks.k1.hdfs.fileType = DataStream
a1.sinks.k1.hdfs.writeFormat = Text
a1.sinks.k1.hdfs.rollInterval = 86400
a1.sinks.k1.hdfs.rollSize = 1000000
a1.sinks.k1.hdfs.rollCount = 10000
a1.sinks.k1.hdfs.idleTimeout = 0

# 配置Channel
a1.channels.c1.type = memory
a1.channels.c1.capacity = 1000
a1.channels.c1.transactionCapacity = 100

# 将三者连接
a1.sources.r1.channel = c1
a1.sinks.k1.channel = c1

七、以上文件设置了Source、Channel和Sink，将Nginx日志中的记录采集到HDFS，运行
flume-ng agent -n a1 -c conf -f $FLUME_HOME/conf/flume-conf.properties
如果没有报错，则安装设置成功了，Nginx中新增加的记录都会被Flume采集，并且存储到HDFS。
