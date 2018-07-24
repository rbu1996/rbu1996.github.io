---
layout: post
title: SQL Server AlwaysOn - 1 (Windows Server 2012, Driver, Remote Control)
date: 2018-07-19 13:45:34.000000000 +09:00
published: true
---

## Preparation Work
1. Download Windows Server 2012 R2, evaluation file type ISO
[Official website](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2012-r2/)
2. Format a U disk
3. [Download UltralISO](https://cn.ultraiso.net)
4. Use UltralISO to change the U disk to startup disk

## Install the Windows Server 2012
Step by step
![install-ws-1.jpg]({{ "/assets/images/install-ws-1.jpg" | absolute_url }})
![install-ws-2.jpg]({{ "/assets/images/install-ws-2.jpg" | absolute_url }})
![install-ws-3.jpg]({{ "/assets/images/install-ws-3.jpg" | absolute_url }})
![install-ws-4.jpg]({{ "/assets/images/install-ws-4.jpg" | absolute_url }})
![install-ws-5.jpg]({{ "/assets/images/install-ws-5.jpg" | absolute_url }})
![install-ws-6.jpg]({{ "/assets/images/install-ws-6.jpg" | absolute_url }})
![install-ws-7.jpg]({{ "/assets/images/install-ws-7.jpg" | absolute_url }})
![install-ws-8.jpg]({{ "/assets/images/install-ws-8.jpg" | absolute_url }})
![install-ws-9.jpg]({{ "/assets/images/install-ws-9.jpg" | absolute_url }})
![install-ws-10.jpg]({{ "/assets/images/install-ws-10.jpg" | absolute_url }})
Network configure
![internet-1.jpg]({{ "/assets/images/internet-1.jpg" | absolute_url }})
![internet-2.jpg]({{ "/assets/images/internet-2.jpg" | absolute_url }})
![internet-3.jpg]({{ "/assets/images/internet-3.jpg" | absolute_url }})

## Install drivers 
### LSI
[Download location](https://www.broadcom.com/products/storage/host-bus-adapters/sas-9300-8i)

Control Panel --> System and Security --> System --> Device Manager
![LSI-driver-1.jpg]({{ "/assets/images/LSI-driver-1.jpg" | absolute_url }})
Properties --> Update Driver --> Search for driver software in this location
![LSI-driver-2.jpg]({{ "/assets/images/LSI-driver-2.jpg" | absolute_url }})
Success!
![LSI-driver-3.jpg]({{ "/assets/images/LSI-driver-3.jpg" | absolute_url }})

### Ethernet
[Download location](https://downloadcenter.intel.com/zh-cn/download/21694/Ethernet--Windows-Server-2012-)

Begin installation
![ethernet-driver-1.jpg]({{ "/assets/images/ethernet-driver-1.jpg" | absolute_url }})
Success!
![ethernet-driver-2.jpg]({{ "/assets/images/ethernet-driver-2.jpg" | absolute_url }})

### NVMe
[Download location](https://downloadcenter.intel.com/zh-cn/download/27517?v=t)

Begin installation
![nvme-driver-1.jpg]({{ "/assets/images/nvme-driver-1.jpg" | absolute_url }})

### Board
[Download location](https://downloadcenter.intel.com/zh-cn/download/27957/-Windows-)

Begin installation
![board-driver-1.jpg]({{ "/assets/images/board-driver-1.jpg" | absolute_url }})


## Some problems in Installation 
### Windows Remote Desktop
#### Server:
Control Panel --> System and Security --> System --> Remote Settings
![remote-desktop-1.jpg]({{ "/assets/images/remote-desktop-1.jpg" | absolute_url }})
Select Users
![remote-desktop-2.jpg]({{ "/assets/images/remote-desktop-2.jpg" | absolute_url }})
Add
![remote-desktop-3.jpg]({{ "/assets/images/remote-desktop-3.jpg" | absolute_url }})
![remote-desktop-4.jpg]({{ "/assets/images/remote-desktop-4.jpg" | absolute_url }})
![remote-desktop-5.jpg]({{ "/assets/images/remote-desktop-5.jpg" | absolute_url }})

#### PC:
Open the Remote Desktop Connection
![remote-desktop-6.jpg]({{ "/assets/images/remote-desktop-6.jpg" | absolute_url }})
![remote-desktop-7.jpg]({{ "/assets/images/remote-desktop-7.jpg" | absolute_url }})
Success!
![remote-desktop-8.jpg]({{ "/assets/images/remote-desktop-8.jpg" | absolute_url }})

#### Error: 
error: An authentication error has occurred. The function requested is not supported.
![remote-desktop-error.jpg]({{ "/assets/images/remote-desktop-error.jpg" | absolute_url }})
Microsoft official docs: edit Credentials Delegation
Group Policy -> Computer Configuration -> Administrative Templates -> System -> Credentials Delegation> Encrypted Oracle Remediation change to Vulnerable (Vulnerable – Client applications that use CredSSP will expose the remote servers to attacks by supporting fallback to insecure versions, and services that use CredSSP will accept unpatched clients.)

### Windows cannot be installed to this disk
Call cmd.exe
```
shift + f10
```
Input
```
diskpart
```
Enter DISKPART, show all disks.
```
list disk
```
Select a disk, and clean and format the selected disk.
```
select disk 0
```
```
clean
```
Set up mbr disk partition.
```
convert mbr
```

