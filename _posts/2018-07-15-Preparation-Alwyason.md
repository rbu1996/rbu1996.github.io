---
layout: post
title: 一些关于Windows Server，SQL Server Always On的笔记
date: 2018-07-15 10:42:13.000000000 +09:00
published: true
---

##	Windows Server 2012 R2
[Official Documents](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh801901(v%3dws.11))

### Storage Space
  Storage Spaces——为通过集群工业标准硬盘到存储池然后在这些存储池中从已有容量中创造存储“空间”，以此实现存储虚拟化。
	What is Storage space: Storage Spaces, a technology in Windows and Windows Server that enables you to virtualize storage by grouping industry-standard disks into storage pools, and then creating virtual disks called storage spaces from the available capacity in the storage pools.
	
Two new abstractions:
  Storage pools. A collection of physical disks that enable you to aggregate disks, expand capacity in a flexible manner, and delegate administration.
  Storage spaces. Virtual disks created from free space in a storage pool. Storage spaces have such attributes as resiliency level, storage tiers, fixed provisioning, and precise administrative control.
	个人感觉有点Linux里Logical Volume Manager LVM的意思
	
## SQL Server 2012
### Microft SQL Server 2012是什么
  Microsoft SQL Server 2012是微软发布的新一代数据平台产品，全面支持云技术与平台，并且能够快速构建相应的解决方案实现私有云与公有云之间数据的扩展与应用的迁移。
  
### SQL Server的工作原理
   SQL Server的工作原理：不能直接修改硬盘上的数据，而是先将数据从硬盘读入到内存的data cache，然后在内存中修改（被修改过的页称为脏数据页），最后再从内存回写到硬盘。
（脏页：被修改的数据页）

### 安装 SQL Server 2012 的硬件和软件要求
[官方文档](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2012/ms143506(v=sql.110))

### 个人PC安装
[参考](https://www.cnblogs.com/iLoveBurning/p/7573349.html)

### SQL Server使用
[参考](https://blog.csdn.net/wang20142052/article/details/53461893)

## 高可用性解决方案

### 什么是高可用性
   高可用性解决方案可减少硬件或软件故障造成的影响，保持应用程序的可用性，从而将用户可以察觉到的停机时间减至最少。

### 高可用性可选方案
#### AlwaysOn 故障转移群集实例（FCI）
   作为SQLServer AlwaysOn产品/服务的一部分，AlwaysOn故障转移群集实例利用Windows Server故障转移群集(WSFC)功能通过冗余在实例级别（故障转移群集实例(FCI)）提供了本地高可用性。FC是在Windows Server故障转移群集(WSFC)节点上和（可能）多个子网中安装的单个SQL Server实例。在网络中，FCI显示为在单台计算机上运行的SQL Server实例，不过它提供了从一个WSFC节点到另一个WSFC节点的故障转移（如果当前节点不可用）。

#### AlwaysOn 可用性组 
   是 SQL Server 2012 中引入的企业级高可用性和灾难恢复解决方案，可使一个或多个用户数据库的可用性达到最高。 AlwaysOn 可用性组要求 SQL Server 实例驻留在 Windows Server 故障转移群集 (WSFC) 节点上。

#### 数据库镜像
	
#### 日志传送

## SQL Server高可用方案
### Always On高可用性 解决方案
   SQL Server Always On 即“全面的高可用性和灾难恢复解决方案”。客户通过使用 Always On 技术，可以提高应用程序可用性，并且通过简化高可用性的部署和管理方面的工作。高可用性解决方案可减少硬件或软件故障造成的影响，保持应用程序的可用性，从而将用户可以察觉到的停机时间减至最少。

#### 数据库级可用性
   在同步提交模式下，主副本的数据被同步更新到其他辅助副本，主副本与辅助副本之间可以保持实时同步。当系统监测到主副本发生故障时，辅助副本可以立即成为新的主副本。
	
#### AlwaysOn故障转移集群实例级可用性
   AlwaysOn故障转移集群实例（Failover Cluster Instance，简称 FCI）。可以在多个节点之间进行故障转移。
   在主节点发生故障时，辅助节点提升为主节点并获取共享存储中的数据，然后在这个新的主节点服务器中启动SQL Server服务。
   Tips：辅助节点不从主节点同步数据，数据被保存在共享存储中。
日志传送
   主数据库所做的任何数据变化都会生成事物日志，这些事务日志定期备份。备份文件被辅助数据库所属的实例复制到它的本地文件夹。最后事务日志备份在辅助数据库中进行恢复，实现两个数据库之间异步更新数据。主数据库发生故障时，使用辅助数据库变成联机状态，可以把每个数据库都当作“冷备用”数据库。

### 高可用的服务器配置
   需要Always On高可用方式，故障出现后系统自动切换到备用服务器上，则需要3台（数据库主服务器、监听服务器、从服务器）相同硬件配置和操作系统与补丁、相同数据库版本和补丁的服务器。

### 几种方式的对比
![compare.jpg]({{ "/assets/images/compare.jpg" | absolute_url }})

## Always On
###  两种Always On方式的应用范围
   通过第三方共享磁盘解决方案（SAN）进行的数据保护，使用Always On故障转移群集实例（实例级可用性）
   对通过SQL Server进行的数据保护，使用Always On可用性组（数据库级可用性）。在主数据库和备用副本之间传送数据。有同步提交和异步提交两种方式。
	
### Always On可用性组
   AlwaysOn 可用性组 功能是一个提供替代数据库镜像的企业级方案的高可用性和灾难恢复解决方案。 SQL Server 2012 (11.x) 中引入了 AlwaysOn 可用性组功能，此功能可最大程度地提高一组用户数据库对企业的可用性。 “可用性组” 针对一组离散的用户数据库（称为“可用性数据库” ，它们共同实现故障转移）支持故障转移环境。 一个可用性组支持一组读写主数据库以及一至八组对应的辅助数据库。 （可选）可使辅助数据库能进行只读访问和/或某些备份操作。
   可用性组在可用性副本级别进行故障转移。 故障转移不是由诸如因数据文件丢失而使数据库成为可疑数据库、删除数据库或事务日志损坏等此类数据库问题导致的。
   AlwaysOn 可用性组是在 SQL Server 2012 (11.x) 中引入的高可用性和灾难恢复解决方案，它要求 Windows Server 故障转移群集 (WSFC)。 此外，尽管 AlwaysOn 可用性组 不依赖于 SQL Server 故障转移群集，但您可以使用故障转移群集实例 (FCI) 来为可用性组承载可用性副本。 因此，了解每种群集技术所扮演的角色以及设计您的 AlwaysOn 可用性组 环境所需的注意事项十分重要。
[官方文档](https://docs.microsoft.com/zh-cn/sql/database-engine/availability-groups/windows/availability-modes-always-on-availability-groups?view=sql-server-2017)
	Always On可用性组是SQL Server 2012中引入的企业级高可用性和灾难恢复解决方案，可以使用户数据库的可用性达到最高。

#### 可用性组
   什么是可用性组（Availability Group）针对一组离散的用户数据库（可用性数据库，共同实现故障转移）支持故障转移环境。一个可用性组支持一组数据库以及多组辅助数据库。
    （1）	主副本：用于承载数据库，主副本使一组数据库可用于客户端的读写连接。
    （2）	辅助副本：承载一组辅助数据库，并作为可用组的潜在故障转移目标。主副本将每个主数据库的事物日志发送到每个辅助数据库。每个辅助副本缓存事物日志记录，然后将他们应用到相应的辅助数据库。
    
两种提交方式

#### 同步提交方式
过程：（1）主数据库在将事务日志写入文件的同时就传送给辅助数据库。然后主数据库等待辅助数据库的回应。
		（2）辅助数据库收到了来自主数据库的事物，写入本地事物文件，然后发送确认信息给主数据库。
		（3）主数据库收到辅助数据库发送来的确认信息，结束等待状态，继续运行。
		（4）主数据库在遇到检查点时才将缓存中的“脏页”写回到数据文件；辅助数据库根据收到的事物在本地重做。
		（!）其中的几个名词
		脏页：被修改过的页，脏数据页
		检查点 Check point：checkpoint会搜索整个data cache，将脏页写回到硬盘。

   同步提交的优点：时刻保证有一模一样的副本，有极高的安全性。
   缺点：辅助数据库接收，写入事务日志和发送确认信息这一系列过程有延迟，影响主数据库性能。
   SQL Server Always On可用性组一个主副本最多向两个辅助副本同步提交，其他的使用异步提交。
   
#### 异步提交模式
   优点：主数据库将事务发送给辅助数据库后，不需要等待确认信息，直接运行。因此没有主数据库的等待状态，不影响主数据库性能。
   缺点：辅助数据库可能数据更新失败，导致数据丢失。
   SQL Server最多允许Always On可用性组拥有8个辅助副本，同步不超过2个。

#### 配置过程
[建立活动目录域、DNS服务器和Window故障转移集群](http://www.cnblogs.com/jenrrychen/p/5468269.html)
[Always On可用性组搭建](https://www.cnblogs.com/jenrrychen/p/5471332.html)

## 数据库排名
Oracle、MySQL及Microsoft SQL Serve是绝对的前三名

### Oracle 11g
许可机制：Proprietary
是否SQL：是
优点：Oracle是重要商业项目的首选，同时也是市场上最古老的主流数据库产品。在支持的操作系统方面，Oracle具有最广泛的灵活性

### MySQL
许可机制：开源
是否SQL：是
优点：企业开始时可以使用社区开源版本，然后升级到商业版。可运行在Linux、Windows、OSX。为用户设计数据库提供直观的图形界面。MySQL拥有大量的资料和教程让你开始及处理问题

### Microsoft SQL Server
许可机制：Proprietary
是否SQL：是
使用最多的商业数据库。受限于Windows。
