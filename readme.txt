Flume1.6��װ��Centos6.8����Flume�ɼ�Nginx��־�ϴ���HDFS��
һ������Flume1.6
wget http://archive.apache.org/dist/flume/1.6.0/apache-flume-1.6.0-bin.tar.gz

������ѹ������/usr/localĿ¼��
tar -zxvf apache-flume-1.6.0-bin.tar.gz
mv apache-flume-1.6.0-bin flume
mv flume/ /usr/local

�����޸Ļ�������
export FLUME_HOME=/usr/local/flume
export PATH=$PATH:/sbin:/bin:/usr/sbin:/usr/bin:/usr/X11R6/bin:$JAVA_HOME/bin:$FLUME_HOME/bin

�ġ�ʹ����������Ч
source /etc/profile

�塢���԰�װ���
flume-ng version

�����޸�flume-conf.properties�ļ�
cp flume-conf.properties.template flume-conf.properties
# ����Agent
a1.sources = r1
a1.sinks = k1
a1.channels = c1

# ����Source
a1.sources.r1.type = exec
a1.sources.r1.channels = c1
a1.sources.r1.deserializer.outputCharset = UTF-8

# ������Ҫ��ص���־���Ŀ¼
a1.sources.r1.command = tail -F /usr/local/nginx/log/access.log

# ����Sink
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

# ����Channel
a1.channels.c1.type = memory
a1.channels.c1.capacity = 1000
a1.channels.c1.transactionCapacity = 100

# ����������
a1.sources.r1.channel = c1
a1.sinks.k1.channel = c1

�ߡ������ļ�������Source��Channel��Sink����Nginx��־�еļ�¼�ɼ���HDFS������
flume-ng agent -n a1 -c conf -f $FLUME_HOME/conf/flume-conf.properties
���û�б�����װ���óɹ��ˣ�Nginx�������ӵļ�¼���ᱻFlume�ɼ������Ҵ洢��HDFS��
