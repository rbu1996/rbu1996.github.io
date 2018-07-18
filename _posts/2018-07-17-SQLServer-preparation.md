---
layout: post
title: SQL Server 2012 - preparation
date: 2018-07-17 11:06:08.000000000 +09:00
published: true

---

## Some basic concepts about database

### Which factor decides the performance of database?
The performance depends on disk(storage), instead of CPU.
 
 To prove it, we use the same server but different disks(NVMe, SATASSD. 7.2k HDD, 1.2k HDD)
 
### The ranking of database
1st Oracle: The best choice for enterprise projects. 
            Support kinds of operation system.
            Proprietary.
            But expensive.

2nd MySQL: Open source.
           Support kinds of operation system.
           Cheap.

3rd Microsoft SQL Server: Windows only.
                          Enterprise projects.

### Performance, security and availability
#### Performance
Disks or Storage

#### security
Storage space
Very similar with LVM or RAID in Linux. 

#### Availability 
Servers: Always On

#### HADR
High Availability, Disaster Recovery

## Always On Availability Groups
Always On availability groups is an enterprise-level high-availability and disaster recovery solution introduced in SQL Server 2012 (11.x) to enable you to maximize availability for one or more user databases. Always On availability groups requires that the SQL Server instances reside on Windows Server Failover Clustering (WSFC) nodes. 

## Compare database performance
Different disks, CPU and memory.

