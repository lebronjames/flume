Flume1.6��װ��Centos6.8��
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


