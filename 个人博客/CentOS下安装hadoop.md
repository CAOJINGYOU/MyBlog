# CentOS下安装hadoop

# CentOS下安装hadoop #

## 用户配置 ##

### 添加用户 ###
	adduser hadoop
	passwd hadoop

### 权限配置 ###
	chmod u+w /etc/sudoers
	vi /etc/sudoers
    在
	root    ALL=(ALL) ALL
    下添加
	hadoop    ALL=(ALL) ALL
	chmod u-w /etc/sudoers

## 关闭防火墙 ##

### 查看防火墙状态 ###
	service iptables status

### 关闭防火墙 ###
	service iptables stop

### 查看防火墙开机启动状态 ###
	chkconfig iptables --list

### 关闭防火墙开机启动 ###
	chkconfig iptables off

## 安装JDK1.7 ##

### 卸载系统自带OpenJDK ###

#### 查看目前系统jdk ####
	rpm -qa | grep jdk

#### 得到结果： ####
	java-1.7.0-openjdk-1.7.0.51-2.4.5.5.el7.x86_64
	java-1.7.0-openjdk-headless-1.7.0.51-2.4.5.5.el7.x86_64

#### 卸载： ####
	rpm -e --nodeps java-1.7.0-openjdk-1.7.0.51-2.4.5.5.el7.x86_64
	rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.51-2.4.5.5.el7.x86_64

### 下载JDK1.7 ###
下载地址： [jdk-7u79-linux-x64.tar.gz](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html "jdk-7u79-linux-x64.tar.gz") [下载](http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.tar.gz "下载")

### 上传hadoop文件 ###
使用[Filezilla client](http://https//filezilla-project.org/)把jdk-7u79-linux-x64.tar.gz放到CentOS目录/usr/lib/jvm中([史上最简单的上传文件到linux系统方法](http://jingyan.baidu.com/article/219f4bf7d28185de442d38d2.html))

### 修改权限 ###
	sudo chmod u+x jdk-7u79-linux-x64.tar.gz

### 解压JDK1.7 ###
	cd /usr/lib/jvm
	sudo tar -zxvf ./jdk-7u79-linux-x64.tar.gz  -C /usr/lib/jvm  

### 配置环境 ###

#### 打开profile文件： ####
	sudo gedit /etc/profile

#### 在文件最下边输入： ####
	export JAVA_HOME=/usr/lib/jvm/jdk1.7.0_4579
	export JRE_HOME=/usr/lib/jvm/jdk1.7.0_4579/jre
	export CLASSPATH=.:$JRE_HOME/lib/tr.jar:$JAVA_HOME/lib:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
	export PATH=$JAVA_HOME/bin:$PATH

#### 使其立刻生效： ####
	source /etc/profile

### 验证是否成功 ###
	java -version

### 手动设置系统默认JDK ###
	sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_79/bin/java 300
	sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_79/bin/javac 300
	sudo update-alternatives --install /usr/bin/jar jar /usr/lib/jvm/jdk1.7.0_79/bin/jar 300
	sudo update-alternatives --config java

## 配置SSH免密码登陆 ##

### 进入用户的根目录下 ###
	ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
	cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys

### 验证是否安装成功 ###
	ssh -version

### 登陆 ###
	ssh localhost

## 安装Hadoop2.6.0 ##

### 下载hadoop2.6 ### 
[hadoop-2.6.0.tar.gz](http://mirrors.hust.edu.cn/apache/hadoop/common/stable/hadoop-2.6.0.tar.gz)

### 解压hadoop-2.6.0.tar.gz ###

#### 进入/usr/local/hadoop ####
	sudo tar -zxvf ./hadoop-2.6.0.tar.gz -C /usr/local
	cd /usr/local/
	sudo mv ./hadoop-2.6.0/ ./hadoop
	sudo chown -R hadoop:hadoop ./hadoop

### 配置hadoop2.6.0环境 ###

#### 打开profile ####
	sudo gedit /etc/profile

### 在最下边添加： ###
	# set hadoop path
	export HADOOP_HOME=/usr/local/hadoop
	export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

#### 使其立刻生效： ####
	source /etc/profile

### 修改/usr/local/hadoop/etc/hadoop/hadoop-env.sh ###
	export JAVA_HOME=/usr/lib/jvm/jdk1.7.0_79

## 伪分布式Hadoop2.6.0配置 ##

### etc / hadoop / core-site.xml:
	<configuration>
	    <property>
	        <name>fs.defaultFS</name>
	        <value>hdfs://localhost:9000</value>
	    </property>
	</configuration>

### etc / hadoop / hdfs-site.xml:
	<configuration>
	    <property>
	        <name>dfs.replication</name>
	        <value>1</value>
	    </property>
	</configuration>

## 启动Hadoop2.6.0 ##

### 进入hadoop的bin目录 ###
	cd /usr/local/hadoop/bin

### 格式化Hadoop的文件系统HDFS ###
	hdfs namenode -format

### 进入hadoop的sbin目录 ###
	cd /usr/local/hadoop/sbin

### 启动所有进程 ###
	start-all.sh
成功的话，会看到 successfully formatted 的提示，且倒数第5行的提示如下，Exitting with status 0 表示成功，若为 Exitting with status 1 则是出错。若出错（不该如此，请仔细检查之前步骤），可试着加上 sudo, 既 sudo bin/hdfs namenode -format 再试试看。

### 关闭命令 ###
	stop-all.sh

### 用jps查看启动的进程 ###
	Jps
	ResourceManager
	NameNode
	DataNode
	SecondaryNameNode
	NodeManager

## 浏览器访问：http://localhost:50070  ##

## hadoop进程管理页面http://localhost:8088 ##

## 问题 ##
 	WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable

原因是hadoop-2.6.0.tar.gz安装包是在32位机器上编译的，64位的机器加载本地库.so文件时出错，不影响使用。

### 解决： ###
	1、重新编译源码后将新的lib/native替换到集群中原来的lib/native
	2、修改hadoop-env.sh ，增加
	export HADOOP_OPTS="-Djava.library.path=$HADOOP_PREFIX/lib:$HADOOP_PREFIX/lib/native"

----------

#### 文档参考： ####
1. [hadoop 2.6.0单节点-伪分布式模式安装](http://www.aboutyun.com/thread-10554-1-1.html)
1. [Hadoop:设置一个节点集群](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/singlecluster.html)
1. [Hadoop安装教程_单机/伪分布式配置_Hadoop2.6.0/Ubuntu14.04](http://www.powerxing.com/install-hadoop/)
1. [Hadoop入门基础教程之服务器基础环境搭建](http://www.linuxidc.com/linux/2015-03/114669.htm)
 