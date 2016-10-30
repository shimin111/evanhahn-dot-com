---
title: Hadoop集群配置以及运行官方示例
layout: post
permalink: /hadoop-setup-and-run-some-native-example-on-it/
---
这篇博客的主要内容是配置hadoop集群以及在hadoop集群上运行官方提供的一些程序，主要目的是hadoop入门。由于本人在配置过程中遇到了坑，所有觉得有必要提醒后来者。我的安装环境是ubuntu server 16.04。下面分为两个部分：1.hadoop集群搭建；2.运行官方提供的例子程序。

# Hadoop集群搭建

## 安装java
JAVA环境是必需的，自己安装没问题，只要注意设置豪环境变量就可以。可以参考[java环境安装](http://www.jianshu.com/p/6c308f184b61)

## 安装hadoop

> 在安装hadoop前，我的vmware虚拟机中已有三台ubuntu server的主机，且主机名分别为ubuntu1，ubuntu2，ubuntu3。三台机器在同一个子网内，相互可以ping通（最好关闭主机的防火墙），三台机子的ip地址分别为192.168.64.128，192.168.64.129，192.168.64.130  

主机名可以自己修改，在root权限下修改`/etc/hostname`即可。输入命令：

    $ hostname

就可以看到
在master节点上，使用下面的命令下载并且安装Hadoop（注意我讲Hadoop下载并且安装在`/usr/local/hadoop/`，可以自行选择合适的目录）

    $ cd /usr/local/
    $ mkdir hadoop;cd hadoop
    $ wget www-eu.apache.org/dist/hadoop/common/hadoop-2.7.3/hadoop-2.7.3.tar.gz
    $ tar -zxvf hadoop-2.7.3.tar.gz
    $ rm hadoop-2.7.3.tar.gz
    $ cd hadoop-2.7.3

## 配置hadoop集群文件
配置文件在`/usr/local/hadoop/hadoop-2.7.3/etc/hadoop/`目录下。
打开并修改`core-site.xml`文件并修改如下：

    <configuration>
        <property>
                <name>fs.defaultFS</name>
                <value>hdfs://ubuntu1:8020/</value>
        </property>
        <property>
                <name>hadoop.tmp.dir</name>
                <value>/usr/local/hadoop/hadoop-2.7.3/tmp</value>
        </property>
    </configuration>

打开`hdfs-site.xml`文件并修改如下：

    <configuration>
        <property>
                <name>dfs.datanode.data.dir</name>
                <value>/usr/local/hadoop/hadoop-2.7.3/dfs/data</value>
                <final>true</final>
        </property>
        <property>
                <name>dfs.namenode.name.dir</name>
                <value>/usr/local/hadoop/hadoop-2.7.3/dfs/name</value>
                <final>true</final>
        </property>
        <property>
                <name>dfs.replication</name>
                <value>2</value>
        </property>
    </configuration>

打开`mapred-site.xml`文件并修改如下：

    <configuration>
        <property>
                <name>mapred.jog.tracker</name>
                <value>ubuntu1:9001</value>
        </property>
    </configuration>

打开`hadoop-env.sh`文件并设置`JAVA_HOME`，将`jdk`的安装路径导入，命令如下：

    export JAVA_HOME=/usr/lib/jvm/java-7-oracle    (就是上文中java的安装位置)


## 配置hosts
接下来一步很关键，我自己在配置的时候，就是这一步出现了问题，在`ROOT`权限`/etc/host`配置主机名-ip映射。

    $ sudo vim /etc/hosts

在host文件的前面添加三对信息，修改后如下：

    192.168.64.128 ubuntu1
    192.168.64.129 ubuntu2
    192.168.64.130 ubuntu3
    # 添加的信息在上面
    127.0.0.1       localhost
    127.0.1.1       ubuntu1
    
    # The following lines are desirable for IPv6 capable hosts
    ::1     localhost ip6-localhost ip6-loopback
    ff02::1 ip6-allnodes
    ff02::2 ip6-allrouters

如果将这三对信息添加在`127.0.1.1`下面的话，master主机(即ubuntu1)在监听时使用的是`127.0.1.1:8020`而不是`192.168.64:8020`。在这种情况下，ubuntu2和ubuntu3上的两个datanode节点无法与ubuntu1上的namenode正常通信。这是ubuntu特有的情况，如果没有这些问题当然更好。

>可以在ubuntu1和ubuntu2上使用命令
>    $ telnet 192.168.64.128 8020
>查看192.168.64.128:8020是否连通
>或者在ubuntu1下输入命令查看正在监听的8020端口情况
>    $ netstat -an | grep 8020
> 查看监听的ip是否是`192.168.64.128`而不是`127.0.1.1`

## 配置master和slaves信息
当前目录为`/usr/local/hadoop/hadoop-2.7.3/`，输入命令：

    $ vim etc/hadoop/masters
    ubuntu1

保存并退出。输入类似命令：

    $ vim etc/hadoop/slaves
    ubuntu2
    ubuntu3

保存并退出。

## 在slave主机安装Hadoop
在master主机（即ubuntu1）输入以下命令，将配置好的hadoop分发给其余的两个主机ubuntu2和ubuntu3。

    $ cd /usr/local/hadoop/
    $ scp -r hadoop-2.7.3 ubuntu2:/usr/local/hadoop/
    $ scp -r hadoop-2.7.3 ubuntu3:/usr/local/hadoop/

至此集群的基本工作完成。

##Format Master上的Namenode以及启动hadoop服务
输入以下命令，format master上的nanenode:

    $ cd /usr/local/hadoop/hadoop-2.7.3/
    $ bin/hadoop namenode -format

上述命令完成后，输入以下命令启动hadoop服务：

    $ sbin/start-all.sh

至此，hadoop集群已经成功的运行起来了，在`http://192.168.64.128:50070/`查看namenode及datanode的连接，应该注意到livenode的数目为2。
关闭所有的进程并退出可以输入命令：

    $ sbin/stop-all.sh

关闭所有的进程。

# 运行官方提供的示例程序

## Word Count
WordCount是hadoop官方提供的示例程序，我在网上找到一份中文的文档，里面有源码的解读，可参考[mapred-tutorial](http://hadoop.apache.org/docs/r1.0.4/cn/mapred_tutorial.html)。在这里可以了解以下map-reduce的原理和机制。
运行官方的例子则相对简单，在我们下载的hadoop包里，即目录`/usr/local/hadoop/hadooop-2.7.3/share/hadoop/mapreduce/`有一个jar包`hadoop-mapreduce-examples-2.7.3.jar`打包的就是官方的示例，可以直接提交给hadoop集群运行。
以wordcount为例，首先准备程序的输入，在hdfs中创建文件夹以及创建输入文件，输入以下命令：

    $ cd /usr/local/hadoop/hadoop-2.7.3
    $ mkdir input (存放临时输入文件)
    $ echo -e Hello World Bye World > input/file01.txt
    $ echo -e Hello Hadoop Goodbye Hadoop > input/file02.txt
    $ bin/hadoop fs -mkdir hdfs://ubuntu1:8020/wordcount
    $ bin/hadoop fs -mkdir hdfs://ubuntu1:8020/wordcount/input
    $ bin/hadoop fs -cp file:///usr/local/hadoop/hadoop-2.7.3/input/file01.txt hdfs://ubuntu1:8020/wordcount/input/
    $ bin/hadoop fs -cp file:///usr/local/hadoop/hadoop-2.7.3/input/file02.txt hdfs://ubuntu1:8020/wordcount/input/

至此，输入文件已经准备好了，可以提交wordcount程序给hadoop集群运行了。命令如下：

    $ cp share/hadoop/map
    $ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.3.jar wordcount hdfs://ubuntu1:8020/wordcount/input/ hdfs://ubuntu1:8020/wordcount/output/

查看输出结果的命令及结果是：

    $ bin/hadoop fs -cat hdfs://ubuntu1:8020/wordcount/output/part-0000
    Bye 1
    GoodBye 1
    Hadoop 2
    Hello 2
    World 2


## PI计算
PI计算同理，但是PI计算不需要准备输入文件，命令如下：

    $ bin/hadoop share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.3.jar pi 100 10000

命令执行完毕即可看到模拟的圆周率,上述命令表明有100个Map作业，每个作业有10000个抽样。