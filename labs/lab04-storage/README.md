# Lab N ─ 04



Machine: DESKTOP-EA7P1RT, lab01, lab02

Date: 06.11.2026



## What i did:



1. Added three 2GB virtual disks (lab4-a.vmdk, lab4-b.vmdk, lab4-spare.vmdk) to Linux VM
2. Created a snapshot (b4 breaking) before breaking anything
3. Added one 2GB virtual disk (lab4-win-a.vmdk) to Windows VM
4. Created a snapshot (snapshot w disk) just in case
5. Get-Disk
6. Initialize-Disk -Number 1 -PartitionStyle GPT
7. New-Partition -DiskNumber 1 -UseMaximumSize -DriveLetter E
8. Format-Volume -DriveLetter E -FileSystem NTFS -NewFileSystemLabel LAB4
9. Get-Disk
10. Get-Partition -DiskNumber 1
11. $env:firmware\_type
12. mkdir -p \~/lab04/evidence
13. cd \~/lab04/evidence
14. lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINT > lsblk-before.txt
15. cat lsblk-before.txt
16. mdadm --version
17. sudo parted /dev/sdb --script mklabel gpt mkpart lab4 1MiB 100% set 1 raid on
18. sudo parted /dev/sdc --script mklabel gpt mkpart lab4 1MiB 100% set 1 raid on
19. lsblk /dev/sdb /dev/sdc
20. sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1
21. cat /proc/mdstat
22. date > array-healthy.txt
23. cat /proc/mdstat >> array-healthy.txt
24. sudo mdadm --detail /dev/md0 >> array-healthy.txt
25. sudo mkfs.ext4 -L lab4 /dev/md0
26. sudo mkdir -p /mnt/mirror
27. sudo mount /dev/md0 /mnt/mirror
28. sudo chown $USER /mnt/mirror
29. echo "Lab 4 - <your surname> - $(date)" > /mnt/mirror/proof.txt
30. dd if=/dev/urandom of=/mnt/mirror/random.bin bs=1M count=200
31. sync
32. sha256sum /mnt/mirror/proof.txt /mnt/mirror/random.bin > checksum-before.txt
33. cat checksum-before.txt
34. sudo mdadm /dev/md0 --fail /dev/sdc1
35. date > array-degraded.txt
36. cat /proc/mdstat >> array-degraded.txt
37. sudo mdadm --detail /dev/md0 >> array-degraded.txt
38. sudo mdadm /dev/md0 --remove /dev/sdc1
39. sudo umount /mnt/mirror
40. sudo mount /dev/md0 /mnt/mirror
41. sha256sum /mnt/mirror/proof.txt /mnt/mirror/random.bin > checksum-after-failure.txt
42. cat /mnt/mirror/proof.txt
43. diff checksum-before.txt checksum-after-failure.txt \&\& echo MATCH
44. sudo parted /dev/sdd --script mklabel gpt mkpart lab4 1MiB 100% set 1 raid on
45. sudo mdadm /dev/md0 --add /dev/sdd1
46. watch -n 1 cat /proc/mdstat
47. date > array-rebuilt.txt
48. cat /proc/mdstat >> array-rebuilt.txt
49. sudo mdadm --detail /dev/md0 >> array-rebuilt.txt
50. lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINT



## Result:

## Windows

Disk 0: partition style GPT, firmware\_type UEFI — because UEFI firmware requires a GPT partition style to boot modern operating systems.

New disk: partition style GPT — because it is standard practice, even though 2 GB fits under both MBR and GPT.

Output of Get-Disk:

PS C:\\Users\\arrrs> Get-Disk



Number Fri Serial Number                    HealthStatus         OperationalStatus      Total Size Partition

&#x20;      end                                                                                         Style

&#x20;      ly

&#x20;      Nam

&#x20;      e

\------ --- -------------                    ------------         -----------------      ---------- ----------

0      VMw 18ED\_A3A5\_33CA\_3F92\_000C\_2961... Healthy              Online                      64 GB GPT

1      VMw AB53\_847B\_06BB\_AC14\_000C\_296D... Healthy              Online                       2 GB GPT



Output of Get-Partition:

PS C:\\Users\\arrrs> Get-Partition -DiskNumber 1





&#x20;  DiskPath:

\\\\?\\scsi#disk\&ven\_nvme\&prod\_vmware\_virtual\_n#5\&25a13950\&0\&000001#{53f56307-b6bf-11d0-94f2-00a0c91efb8b}



PartitionNumber  DriveLetter Offset                                   Size Type

\---------------  ----------- ------                                   ---- ----

1                           17408                                15.98 MB Reserved

2                E           16777216                              1.98 GB Basic



## Linux layout — before



NAME                       SIZE TYPE FSTYPE      MOUNTPOINT

loop0                      1.5M loop squashfs    /snap/hwctl/123

loop1                      1.5M loop squashfs    /snap/hwctl/131

loop2                     66.8M loop squashfs    /snap/core24/1643

loop3                     50.1M loop squashfs    /snap/snapd/27710

sda                         20G disk

├─sda1                       1M part

├─sda2                     1.8G part ext4        /boot

└─sda3                    18.2G part LVM2\_member

&#x20; └─ubuntu--vg-ubuntu--lv   10G lvm  ext4        /

sdb                          2G disk

sdc                          2G disk

sdd                          2G disk

sr0                        2.7G rom  iso9660



## Linux layout — after the rebuild



NAME                       SIZE TYPE  FSTYPE            MOUNTPOINT

loop0                      1.5M loop  squashfs          /snap/hwctl/123

loop1                      1.5M loop  squashfs          /snap/hwctl/131

loop2                     66.8M loop  squashfs          /snap/core24/1643

loop3                     50.1M loop  squashfs          /snap/snapd/27710

sda                         20G disk

├─sda1                       1M part

├─sda2                     1.8G part  ext4              /boot

└─sda3                    18.2G part  LVM2\_member

&#x20; └─ubuntu--vg-ubuntu--lv   10G lvm   ext4              /

sdb                          2G disk

└─sdb1                       2G part  linux\_raid\_member

&#x20; └─md0                      2G raid1 ext4              /mnt/mirror

sdc                          2G disk

└─sdc1                       2G part  linux\_raid\_member

sdd                          2G disk

└─sdd1                       2G part  linux\_raid\_member

&#x20; └─md0                      2G raid1 ext4              /mnt/mirror

sr0                        2.7G rom   iso9660



|Role|Device|
|-|-|
|Mirror half A|sdb1|
|Failed half|sdc1|
|Replacement|sdd1|



## The Question

Your colleague deleted the wrong file from the mirror an hour ago.

Does RAID 1 help? Why?

No, RAID 1 does not help if the colleague deletes wrong file. A mirror instantly copies every change to both disks, so the file is immediately removed from the entire array. Instead, a proper backup or a file snapshot is what would have helped to recover the deleted data.



## What did  not work the first time:

Half of the commands for Linux VM did not work and gave "No such file or directory" message. It happened because of typos (sudo parted /dev/sdd --scripts... sha356sum... etc) that are easy to miss, and inability to copy-paste commands in the ubuntu terminal. Double checked every command before pressing enter.



## Evidence:

evidence/lsblk-before.txt - initial block device layout before partitioning

evidence/array-healthy.txt - details of the fully working RAID 1 mirror

evidence/array-degraded.txt - details of the RAID 1 array after failing one disk

evidence/checksum-before.txt - sha256 checksums of files written to the healthy mirror

evidence/checksum-after-failure.txt - sha256 checksums verified from the surviving disk

evidence/array-rebuilt.txt - details of the RAID 1 array after rebuilding onto the spare disk

