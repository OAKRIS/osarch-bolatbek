# Task 2 — Plan and justify



## Allocation:

|Guest|Ubuntu|Windows|
|-|-|-|
|vCPU|1-2|2|
|memory|2GB|4GB|
|disk size|5GB|65GB|
|disk allocation|dynamic|dynamic|
|network adapters/mode|NAT, VMware NAT|NAT, VMware NAT|



## Arithmetic:
### For the host:
CPU: 0-1

Memory: 2GB

Disk: 5-58GB (Assuming the files are saved on the cloud and the disk space is freed)

My assumption, my own host initially had the same specs. Such allocation is enough for light work on both VMs.


## Verification plan:


Everything is set on a minimum. Giving both VMs 2-3 vCPU is enough for adequate performance, given the total is 4 (0-1 left for host is a good balance). 2 and 4 GB memory allocation is due to Ubuntu Server needing minimal overhead to run the console, and for windows is because it refuses to install below this requirement (same goes for 65GB disk size). Ubuntu disk size is set on the min (5GB) because host has limited disk space and that would have trouble running the disk above set size. Network mode is VMware NAT because it allows guest to guest communication as well as has access to the internet. Dynamic allocation because the file grows only as data is written, as the host has limited storage





#### Resources:

Ubuntu:

* nproc - Guest CPU number
* free -h - Guest memory
* df -g / - Disk size



Windows:

* Get-CimInstance Win32\_Processor | Format-List \* - Guest CPU
* Get-PhysicalDisk | Format-List \* - Guest disk size 
* systeminfo - Guest memory

#### 

#### SSH connection:

ip addr - Ubuntu Guest IP

ipconfig - Windows Guest IP

ping -n \[Ubuntu IP]

ping -c 4 \[Windows IP]



#### Internet access:
Open browser in Windows VM

