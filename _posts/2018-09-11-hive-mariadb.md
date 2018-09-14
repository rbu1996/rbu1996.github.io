---
layout: post
title: Hive &  MariaDB
date: 2018-09-11 12:47:20.000000000 +09:00
published: true
--- 

## 1. MariaDB
#### Install
```
yum -y install mariadb mariadb-server
```
#### Enalble
```
systemctl start mariadb
```
```
systemctl enable mariadb
```
#### Configue
```
mysql_secure_installation
```
```
# If it is your first installation, return
Enter current password for root (enter for none):
# Set root password
Set root password? [Y/n] 
New password: 
Re-enter new password: 
#Other Configuration
Remove anonymous users? [Y/n] 

Disallow root login remotely? [Y/n] 

Remove test database and access to it? [Y/n] 

Reload privilege tables now? [Y/n] 
```
#### Start Mariadb
```
mysql -uroot -ppassword
```

## 2. Hive
==Recommed version==:       
==Hadoop 3.0.3==     
==Hive 3.1.0==          
==Java jdk1.8==             
I used Hive 1.2.2 and jdk 10.0.1 at first, but they do not fit each other.      

### ==NEVER DOWNLOAD THE LEASTEST VERSION==         

```
mkdir /usr/loacl/hive/hive-3.1.0
tar -zxvf hive-3.1.0.tar.gz
```
### 2.1 Configure
```
vim /etc/profile
```
```
export HIVE_HOME=/usr/local/hive/hive-3.1.0
```
```
vim /root/.bashrc
```
```
export HIVE_HOME=/usr/local/hive/hive-3.1.0
export PATH=$HIVE_HOME/bin:
```
```
source /etc/profile
source /root/.bashrc
```
### 2.2 hive-env.sh
```
cp hive-env.sh.template hive-env.sh
vim hive-env.sh
```
```
JAVA_HOME=/usr/local/jdk/jdk-1.8.0
HADOOP_HOME=/usr/local/hadoop/hadoop-3.0.3
export HIVE_CONF_DIR=/usr/local/hive/hive-3.1.0/conf
```
### 2.3 hive-default.xml
```
cp hive-default.xml.template hive-default.xml
vim hive-default.xml
```
```
<property>
   <name>javax.jdo.option.ConnectionURL</name>
   <value>jdbc:mysql://localhost:3306/hive?createDatabaseIfNotExist=true</value>
</property>
<property>
    <name>javax.jdo.option.ConnectionDriverName</name>
    <value>com.mysql.jdbc.Driver</value>
    <description>Driver class name for a JDBC metastore</description>
</property>
<property>
    <name>javax.jdo.option.ConnectionUserName</name>
    <value>root</value>
    <description>Username to use against metastore database</description>
</property>
<property>
    <name>javax.jdo.option.ConnectionPassword</name>
    <value>root</value>
    <description>password to use against metastore database</description>
</property>
<property>
    <name>hive.exec.local.scratchdir</name>
    <value>/usr/local/hive</value>
    <description>Local scratch space for Hive jobs</description>
</property>
<property>
    <name>hive.downloaded.resources.dir</name>
    <value>/usr/local/hive</value>
    <description>Temporary local directory for added resources in the remote file system.
</description>
</property>
```
### Install mariadb-java driver
Download        
[https://downloads.mariadb.com/](https://downloads.mariadb.com/)        
mariadb-java-client-2.3.0.jar       
```
cp mariadb-java-client-2.3.0.jar $HIVE_HOME/lib
```
### Start Hive
```
cd $HIVE_HOME/bin
```
```
schematool
hive
```

