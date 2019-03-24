---
layout: post
published: false
title: mxnet in production
date: '2019-02-27'
subtitle: Try to use mxnet with apache nifi and yarn
---
## Current state

Machine learning in production should have the following architecture. 
 [source](https://www.slideshare.net/Hadoop_Summit/deep-learning-on-yarn-running-distributed-tensorflow-etc-on-hadoop-cluster-v3)

![deep-learning-on-yarn-running-distributed-tensorflow-etc-on-hadoop-cluster-v3-11-1024.jpg]({{site.baseurl}}/img/deep-learning-on-yarn-running-distributed-tensorflow-etc-on-hadoop-cluster-v3-11-1024.jpg)


Currently my work is mainly forcus on training data ( the last step)

- Use mxnet to train serveral model
- Use mxboard + tensorboard to create train+val log
- Use mxnet model server to host trained model. The rest API is public

For learning purpose, i tried to implement the above architecture. 
Using Apache Hadoop Submarine [document](https://hadoop.apache.org/docs/r3.2.0/hadoop-yarn/hadoop-yarn-applications/hadoop-yarn-submarine/QuickStart.html) .



## TODO

1.

## Quick note

Read some concept of Apache Hadoop at : [hadoop-tutorial](https://data-flair.training/blogs/hadoop-tutorial/)

Hadoop is the master-slave fashion as shown in below picture. 
When the master node fail, the slave nodes keep working and sending heart-beat signal.
After master node is recorved or replaced, the slave nodes will re-connect again without 
any configuration.

![Hadoop Architecture]({{site.baseurl}}/https://d2h0cx97tjks2p.cloudfront.net/blogs/wp-content/uploads/sites/2/2018/01/Hadoop-Architecture.png)

Hadoop use the HDFS (Hadoop distributed file system) and MapReduce. 

To manage resource for Hadoop, we use YARN ( Yet another resource negoliation) .  

The ecosystem on the above tutorial does not contain apache nifi.
See [flume vs nifi](https://dzone.com/articles/big-data-ingestion-flume-kafka-and-nifi) for a quick comparision.


Hence, in conclusion to this Big Data tutorial, we can say that Apache Hadoop is the most popular and powerful big data tool. Big Data stores huge amount of data in the distributed manner and processes the data in parallel on a cluster of nodes. It provides the world’s most reliable storage layer- HDFS. Batch processing engine MapReduce and Resource management layer- YARN. 4 daemons (NameNode, datanode, node manager, resource manager) run in Hadoop to ensure Hadoop functionality.


## Install hadoop + yarn

### Install hadoop for 1 machine (master node only)

There're a lot of tutorial on the web. But the only tutorial that work is [official](https://hadoop.apache.org/docs/r3.0.3/hadoop-project-dist/hadoop-common/SingleCluster.html)

The detail is as follow

```bash
1. Add to  bash.rc
# Set Hadoop-related environment variables
export HADOOP_HOME=/home/jil/opt/hadoop-3.1.1
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
# Add Hadoop bin/ directory to PATH
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin

2. Download hadoop
tar xfv hadoop-3.1.1.tar.gz 
cd hadoop-3.1.1/

3. Backup etc
cp -r etc/hadoop etc/hadoop_def

4. Add the following settings

### core-site.xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>

### hdfs-site.xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>

### marped-site.xml
<configuration>
    <property>
        <name>mapreduce.application.classpath</name>
        <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
    </property>
</configuration>

### yarn-site.xml
<configuration>

<!-- Site specific YARN configuration properties -->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property>
</configuration>

5. then start dfs, yarn
# reset on the data
hdfs namenode -format
sbin/start-dfs.sh
sbin/start-yarn.sh

6. Final

Resource management : http://localhost:8088/cluster 
namenode: http://localhost:9870/

```

### Install hadoop for clusters ( master + cluster machines)

Later when we have more machine .
See [tutorial point](https://www.tutorialspoint.com/hadoop/hadoop_multi_node_cluster.htm)
 fore more detail




### TODO

[apache submarine title 1](https://hortonworks.com/blog/submarine-running-deep-learning-workloads-apache-hadoop/)

[apache submarine official document](https://hadoop.apache.org/docs/r3.2.0/hadoop-yarn/hadoop-yarn-applications/hadoop-yarn-submarine/QuickStart.html) 


[Hadoop on single node](https://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/)

[Install hadoop (tutorial point) ](https://www.tutorialspoint.com/hadoop/hadoop_multi_node_cluster.htm) ---> only this work


### Install Zeppelin

Go to [zeppelin page](http://zeppelin.apache.org/) . We can use anaconda inside zeppelin. so the installation should be as follow

```bash
1. Download anaconda
wget https://repo.anaconda.com/miniconda/Miniconda2-latest-Linux-x86_64.sh

2. Instal at /opt/hadoop/anaconda

3. Reload the value on $PATH environment
source ~/.bashrc

Note: Zeppelin load the value of $PATH, so if we do not update the $PATH correctly, conda will not be available in the zeppelin. 

Remember to print the $PATH to check whether conda is available inside the $PATH. Example is as follow: 
/opt/hadoop/miniconda2/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

4. Download zeppelin
http://ftp.jaist.ac.jp/pub/apache/zeppelin/zeppelin-0.8.1/zeppelin-0.8.1-bin-all.tgz

5. Install zeppelin
tar zeppelin-0.8.1-bin-all.tgz 
ln -s /opt/hadoop/zeppelin-0.8.1-bin-all /opt/hadoop/zeppelin

6. Start zeppelin
bin/zeppelin-daemon.sh start

The web interface of zeppelin should be at: http://gpu01:8080/#/

7. Edit interpreters
Go to "Interpreter" (below About Zeppelin). 
Edit python interpreter
    zeppelin.python	            /opt/hadoop/miniconda2/bin/python
    zeppelin.python.maxResult	1000
Edit spark interpreter
    zeppelin.pyspark.python	    /opt/hadoop/miniconda2/bin/python

Then save and anaconda should be available

8. Chech whether anaconda work inside zeppelin notebook

%python.conda info

    Conda Information



     active environment : base
    active env location : /opt/hadoop/miniconda2
            shell level : 2
       user config file : /home/hadoop/.condarc
 populated config files : 
          conda version : 4.6.7
    conda-build version : not installed
         python version : 2.7.15.final.0
       base environment : /opt/hadoop/miniconda2  (writable)
           channel URLs : https://repo.anaconda.com/pkgs/main/linux-64
                          https://repo.anaconda.com/pkgs/main/noarch
                          https://repo.anaconda.com/pkgs/free/linux-64
                          https://repo.anaconda.com/pkgs/free/noarch
                          https://repo.anaconda.com/pkgs/r/linux-64
                          https://repo.anaconda.com/pkgs/r/noarch
          package cache : /opt/hadoop/miniconda2/pkgs
                          /home/hadoop/.conda/pkgs
       envs directories : /opt/hadoop/miniconda2/envs
                          /home/hadoop/.conda/envs
               platform : linux-64
             user-agent : conda/4.6.7 requests/2.21.0 CPython/2.7.15 Linux/4.4.0-142-generic ubuntu/16.04.5 glibc/2.23
                UID:GID : 1002:1002
             netrc file : None
           offline mode : False
検知をするための特徴抽出層からなる。ベース部分では物体の認知に関する学習を行い
9. Stop zeppelin
bin/zeppelin-daemon.sh stop

```


TODO: based on https://github.com/hadoopsubmarine/hadoop-submarine-ecosystem/issues/4#issuecomment-468198256 , zeppelin's submarine interpreter will be available on March 20th.
So currently stop here and try later in the future. 


