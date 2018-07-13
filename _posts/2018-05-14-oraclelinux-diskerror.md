---
layout: post
title: Install Oracle Linux 7.4 - Problem with your disk selection
date: 2018-05-14 17:21:12.000000000 +09:00
published: true
---

## ERROR: Problem with your disk selection 
### Error
When I installed OS in one server, I got back this error message: 
There was a problem with your disk selection. Click here for details. 

![cannot-select-disk-error-1.jpg]({{ "/assets/images/cannot-select-disk-error-1.jpg" | absolute_url }})

Select the disk as it required. Begin installation. 

### Check the disk information 
List all available block device information, and display dependencies among them, but it does not list information on RAM disk.
```shell
lsblk
```

![disk-error-solution-1.jpg]({{ "/assets/images/disk-error-solution-1.jpg" | absolute_url }})

Find the problem! Someone used this server as DAID before, so I was not able to select only one disk as OS disk. 

### Delete RAID
```shell 
for i in {0..3};do dd if=/dev/zero of=/dev/nvme"$i"n1 seek=78100000 bs=512 count=422768; done
```

![disk-error-solution-2.jpg]({{ "/assets/images/disk-error-solution-2.jpg" | absolute_url }})

After deleting the RAID, we can choose the OS disk only~

## Another way
### BIOS
Enter BIOS
Delete RAID

ummmmm, I forgot to do the screen shoot.
