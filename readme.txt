Flume1.6安装（Centos6.8）
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


