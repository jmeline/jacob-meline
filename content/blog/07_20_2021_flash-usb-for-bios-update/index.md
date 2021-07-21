---
title: Flash usb for updating motherboard bios in linux
date: "2021-07-20"
description: "Learn how to script the updating a motherboard bios"
---

From time to time I like to update my motherboard's bios to the latest version and I would like to script it out. 

There are always a couple of steps that I forget, hence the blog post.

Usually this task is best done via ```fdisk```, ```gdisk```, or through one of the many [tools](https://wiki.archlinux.org/title/partitioning#Partitioning_tools) out there. Scripting can be a recipe for destruction of your data. 

**You have been warned**

sfdisk is the scriptable equivalent of fdisk and it only takes arguments via STDIN. I'll demonstrate how it works.

## Partition usb drive in preparation 
My 4GB flash drive is located at ```/dev/sdb```

First, I'll delete the partition. 

```
# sudo sfdisk --delete /dev/sdb
```

Next, I'll need to create a FAT32 usb to hold my new motherboard bios

```
# echo ',,c;' | sudo sfdisk /dev/sdb
```
This will create one partition. It'll start at the first sector and take up the entire drive. 'c' is the label for W95 FAT 32 (LBA). This is the recommended type. 

This step is important because your motherboard most likely won't pickup that the usb is in FAT32 format. Even when I formatted the drive as FAT32, because I hadn't set the label on the partition, the motherboard didn't recognize the usb drive as in the acceptable format.

Here is the output:

```
~/c/jacob-meline ❯❯❯ sudo echo ',,c;' | sudo sfdisk /dev/sdb                                                   
Checking that no-one is using this disk right now ... OK

Disk /dev/sdb: 3.66 GiB, 3926949888 bytes, 7669824 sectors
Disk model: DataTraveler G3
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x02ff41b3

Old situation:

Device     Boot Start     End Sectors  Size Id Type
/dev/sdb1        2048 7669823 7667776  3.7G  c W95 FAT32 (LBA)

>>> Created a new DOS disklabel with disk identifier 0xa88cfaca.
/dev/sdb1: Created a new partition 1 of type 'W95 FAT32 (LBA)' and of size 3.7 GiB.
Partition #1 contains a vfat signature.
/dev/sdb2: Done.

New situation:
Disklabel type: dos
Disk identifier: 0xa88cfaca

Device     Boot Start     End Sectors  Size Id Type
/dev/sdb1        2048 7669823 7667776  3.7G  c W95 FAT32 (LBA)

The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

```

## Create the FAT32 filesystem

Warning!! this will delete **everything** on /dev/sdb1 if by chance you had left over data
```
# sudo mkfs.vfat -F 32 /dev/sdb1
```

## Download and Copy bios to usb

I have an Asrock B550 Steel legend. The latest bios is located here: https://download.asrock.com/BIOS/AM4/B550%20Steel%20Legend(2.00)ROM.zip

Download it via curl or something and copy it to the flashdrive.

Reboot into your bios and upgrade your motherboard bios. Remember not to interrupt the UEFI upgrade process.
