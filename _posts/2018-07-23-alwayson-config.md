---
layout: post
title: SQL Server Always On
date: 2018-07-19 13:45:34.000000000 +09:00
published: true
---

## Environment 
|    |Domain Controller|Node 1|Node 2|Node 3|
|Name|WIN-AD           |      |      |
|IP  |172.16.16.100|172.16.16.101|172.16.16.102|172.16.16.103|
|

## Configure Domain Controller
### Domain Controller IP
Open Network and Sharing Center --> Ethernet Status --> Properties --> TCP/IPv4
(Remember to disable IPv6)
DNS server address points to local server
![AD-1.jpg]({{ "/assets/images/AD-1.jpg" | absolute_url }})

### Advanced sharing setting 
Turn on sharing
![AD-2.jpg]({{ "/assets/images/AD-2.jpg" | absolute_url }})

### Add roles and features
![AD-3.jpg]({{ "/assets/images/AD-3.jpg" | absolute_url }})
Add roles and features wizard
![AD-4.jpg]({{ "/assets/images/AD-4.jpg" | absolute_url }})
![AD-5.jpg]({{ "/assets/images/AD-5.jpg" | absolute_url }})
![AD-6.jpg]({{ "/assets/images/AD-6.jpg" | absolute_url }})
Server roles --> Active Directory Domain Service
![AD-7.jpg]({{ "/assets/images/AD-7.jpg" | absolute_url }})
Features --> NET Framework 3.5, Failover Clustering
![AD-8.jpg]({{ "/assets/images/AD-8.jpg" | absolute_url }})
![AD-9.jpg]({{ "/assets/images/AD-9.jpg" | absolute_url }})
![AD-10.jpg]({{ "/assets/images/AD-10.jpg" | absolute_url }})

### Change the Computer name
Control panel --> System and Security --> System --> Change Settings --> Change
![AD-11.jpg]({{ "/assets/images/AD-11.jpg" | absolute_url }})
Restart the server after changing

### Promote the server to a domain controller
![AD-12.jpg]({{ "/assets/images/AD-12.jpg" | absolute_url }})
Active Directory Domain Services Configuration Wizard --> Add a forest
![AD-13.jpg]({{ "/assets/images/AD-13.jpg" | absolute_url }})
Set password
![AD-14.jpg]({{ "/assets/images/AD-14.jpg" | absolute_url }})
Next .....
![AD-15.jpg]({{ "/assets/images/AD-15.jpg" | absolute_url }})

### Check the System
Domain changes to alwayson.sql
![AD-16.jpg]({{ "/assets/images/AD-16.jpg" | absolute_url }})

### Create user and user group
Active Directory Users and Computers --> Users --> Action --> New --> User
![AD-17.jpg]({{ "/assets/images/AD-17.jpg" | absolute_url }})
![AD-18.jpg]({{ "/assets/images/AD-18.jpg" | absolute_url }})
User --> properties --> Number Of --> Add
![AD-19.jpg]({{ "/assets/images/AD-19.jpg" | absolute_url }})
Find Now
![AD-20.jpg]({{ "/assets/images/AD-20.jpg" | absolute_url }})
Add the following numbers
![AD-21.jpg]({{ "/assets/images/AD-21.jpg" | absolute_url }})

### DNS Manager
Check the alwayson.sql, find the win-ad.
![AD-22.jpg]({{ "/assets/images/AD-22.jpg" | absolute_url }})
Done!
