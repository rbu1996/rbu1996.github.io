---
layout: post
title: switch
date: 2018-09-09 11:42:12.000000000 +09:00
published: true
--- 

## 1.Install Scala and Spark
### 1.1 Scala
Spark depends on Scala      
Download location:[http://www.scala-lang.org/](http://www.scala-lang.org/)        
```
mkdir /usr/loacl/scala
tar -zxvf scala-2.12.6.tar.gz
```
### 1.2 Spark
```
mkdir /usr/local/spark
tar -zxvf spark-2.2.2.tar.gz
```
### 1.3 Configure environment variables
(1)
```
vim /etc/profile
```
```
# scala
export SCALA_HOME=/usr/local/scala/scala-2.3

# Spark
export SPARK_HOME=/usr/local/spark/spark-2.2.2
```
```
source /etc/profile
```
(2)
```
vim /root/.bashrc
```
```
# scala
export SCALA_HOME=/usr/local/scala/scala-2.3

# Spark
export SPARK_HOME=/usr/local/spark/spark-2.2.2

export PATH=$SCALA_HOME/bin:$SPARK_HOME/bin
```
```
source /root/.bashrc
```
(3)
```
scala -version
```

## 2. Configure Spark
### 2.1 spark-env.sh
```
cd /usr/loacl/spark/spark-2.2.2/conf
cp spark-env.sh.template spark-env.sh
```
```
vim spark-env.sh
```
Add the following configuration
```
export SCALA_HOME=/usr/local/scala/scala-2.12.6
export JAVA_HOME=/usr/local/jdk/jdk-10.0.2
export HADOOP_HOME=/usr/local/hadoop/hadoop-3.0.3
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export SPARK_HOME=/usr/loacl/spark/spark-2.2.2
```

### 2.2 slaves
```
cd /usr/loacl/spark/spark-2.2.2/conf
cp slaves.template slaves
```
```
vim slaves
```
```
worker1
worker2
worker3
```
## Start Spark
### Start Hadoop
```
cd /usr/loacl/hadoop/hadoop-3.0.3/sbin
./start-all.sh
```
### Start Spark
```
cd /usr/loacl/spark/spark-2.2.2/sbin
./start-all.sh
```

