---
layout: post
title: Hadoop installation
date: 2018-09-03 15:23:59.000000000 +09:00
published: true
--- 

## 1. Environment

Host Name | Role | ip
---|---|---|---
master | 192.168.1.100 | Resource Manager / Name Node / Secondary Name Node
worker1 | 192.168.1.101 | Node Manager / Data Node
worker2 | 192.168.1.102 | Node Manager / Data Node
...|...|...
worker7|192.168.1.107|Node Manager / Data Node

Operation System: Centos 7.5
Hadoop 3.0.3
Java JDK 10.0.2
## 2. Preparation Work
++Do the same in all servers, including master and all workers++
### 2.1 Configure IP
```
vim /etc/hosts
```
Add to the file
```
192.168.1.100   master
192.168.1.101   worker1
192.168.1.102   worker2
...
192.168.1.107   worker7
```
Save to the disk
```
source /etc/hosts
```

### 2.2 Turn Off the Fire Wall
```
systemctl stop firewalld.service
systemctl disable firewalld.service
```

### 2.3 Turn Off the Selinux
```
vim /etc/selinux/config
```
Change the file
```
SELINUX=disable
```
### 2.4 SSH password free login
``` shell
mkdir /root/.ssh
ssh-keygen -t rsa
cd .ssh/
cat id_rsa.pub >> authorized_keys
```
The output of each node's id_rsa.pub (public key) is redirected to authorized_keys.        
scp the authorized_keys of worker7 to all nodes.            

++Check the ssh login in every node++
```
ssh [-l login_name] [-p port] [user@]hostname
ssh root@master
```

## 3. Configure the Environment
++Do the same in all servers, including master and all workers++
### 3.1 Java JDK
++If you have openJDK in you computer, delet it firstly.++      
Download the JDK (newer than 8.0)
```
mkdir /usr/local/jdk
cd /usr/local/jdk
tar -zxvf jdk-10.0.2_linux-x64_bin.tar.gz
```

### 3.2 Hadoop
```
mkdir /usr/local/hadoop
cd /usr/loacl/hadoop
tar -zxvf hadoop-3.0.3.tar.gz
```

### 3.3 Add Hadoop and Java PATH
(1).

```
vim /etc/profile
```
Add the PATH
```
# oracle jdk
export JAVA_HOME=/usr/local/jdk/jdk10.0.2
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH

#hadoop
export HADOOP_HOME=/usr/local/hadoop-3.0.3
export PATH=$PATH:$HADOOP_HOME/bin 
export HADOOP_HOME_WARN_SUPPRESS=1
```
```
source /etc/profile
```
(2).

```
vim /root/.bashrc
```
Add th PATH
```
export HADOOP_HOME = /usr/local/hadoop/hadoop-3.0.3
export PATH = $JAVA_HOME/bin:$HADOOP_HOME/bin:$PATH
```
```
source /root/.bashrc
```



## 4. Configure Hadoop
### 4.1 hadoop-env.sh
```
vim /usr/local/hadoop-3.0.3/etc/hadoop/hadoop-env.sh
```
Add the PATH
```
export JAVA_HOME=/usr/local/jdk/jdk10.0.2
export HADOOP_HOME=/usr/loacl/hadoop/hadoop-3.0.3
```
### 4.2 hdfs-site.xml
```
cd /usr/local/hadoop-3.0.3/etc/hadoop
vim hdfs-site.xml
```
Add the configuration directory 
```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>2</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/hdfs/name</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/hdfs/data1, /hdfs/data2, /hdfs/data3</value>
    </property>
    <property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>pmaster:9001</value>
    </property>
    <property>
        <name>dfs.namenode.checkpoint.dir</name>
        <value>/hdfs/name</value>
    </property>
</configuration>
```
### 4.3 mapred-site.xml
```
cd /usr/local/hadoop-3.0.3/etc/hadoop
vim mapred-site.xml
```
I don't use mapreduce in this testing.

### 4.4 workers
```
cd /usr/local/hadoop-3.0.3/etc/hadoop
vim workers
```
```
worker1
worker2
worker3
worker4
worker5
worker6
worker7
```
### 4.5 yarn-site.xml
```
cd /usr/local/hadoop-3.0.3/etc/hadoop
vim yarn-site.xml
```
```
<configuration>

<!-- Site specific YARN configuration properties -->
     <property>
          <name>yarn.nodemanager.aux-services</name>
          <value>mapreduce_shuffle</value>
     </property>
     <property>
           <name>yarn.nodemanager.aux-services.mapreduce.shuffle.calss</name>
           <value>org.apache.hadoop.mapred.ShuffleHandle</value>
     </property>
     <property>
          <name>yarn.resourcemanager.resource-tracker.address</name>
          <value>master:8025</value>
      </property>
     <property>
         <name>yarn.resourcemanager.resource-tracker.address</name>
         <value>master:8025</value>
     </property>
     <property>
         <name>yarn.resourcemanager.scheduler.address</name>
         <value>master:8030</value>
     </property>
     <property>
         <name>yarn.resourcemanager.address</name>
         <value>master:8040</value>
     </property>
     <property>
          <name>yarn.nodemanager.resource.memory-mb</name>
          <value>98304</value>
     </property>
     <property>
           <name>yarn.scheduler.minimum-allocation-mb</name>
           <value>2048</value>
     </property>
     <property>
          <name>yarn.scheduler.maximum-allocation-mb</name>
          <value>98304</value>
      </property>
     <property>
         <name>yarn.app.mapreduce.am.resource.mb</name>
         <value>2048</value>
     </property>
</configuration>
```

### 4.6 core-site.xml
```
vim core-site.xml
```
```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://master:9000</value>
    </property>
    <property>
        <name>hadoop.tpm.dir</name>
        <value>/usr/loacl/hadoop/hadoop-3.0.3/tmp</value>
    </property>
</configuration>
```

### 4.7 ~/sbin/*.sh
(1)
```
cd /usr/local/hadoop-3.0.3/sbin
vim start-yarn.sh
vim stop-yarn.sh
```
Add the root user
```
YARN_RESOURCEMANAGER_USER=root
HADOOP_SECURE_DN_USER=root
YARN_NODEMANAGER_USER=root
```
(2)
```
vim start-dfs.sh
vim stop-dfs.sh
```
```
HDFS_DATANODE_USER=root
HADOOP_SECURE_DN_USER=root
HDFS_NAMENODE_USER=root
HDFS_SECONDARYNAMENODE_USER=root
```

### ++Copy the hadoop directory to other nodes++

## Mount disks
### Create a new partation
     
(1)     
==++MATSER++== Namenode
```
fdisk /dev/nvme0n1
n
p
1
w
```
```
mkdir /hdfs
cd /hdfs
mkdir /name
mount /dev/nvme0n1p1 /hdfs/name
```
Use 'df' to check the mountpoint
```
df
```
(2)        
==++WORKER++== Datanode
```
fdisk /dev/nvme1n1
n
p
1
w
```
```
mkdir /hdfs
cd /hdfs
mkdir /data1
mount /dev/nvme0n1p1 /hdfs/data1
```
## Start up Hadoop
### Format namenode
```
cd /usr/local/hadoop-3.0.3/bin 
hadoop namenode -format
```

### Start Hadoop
```
cd /usr/local/hadoop-3.0.3/sbin 
./start-all.sh
```
### Cluster Status
```
hadoop dfsadmin -report
```

