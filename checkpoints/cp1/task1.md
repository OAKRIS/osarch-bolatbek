# Task 1 — Inventory and interpret



## 1a. The computer in front of you

|Figure|This computer|Command it came from|
|-|-|-|
|CPU model|Intel(R) Core(TM) i3-N300|Get-CimInstance Win32\_Processor \| Format-List \*|
|Cores/logical processors|8|Get-CimInstance Win32\_Processor \| Format-List \*|
|Hardware virtualization: supported? enabled in firmware?|Yes|systeminfo|
|Memory: total / available now|Полный объем физической памяти:   7 874 МБ<br />|systeminfo|
|System disk: size, free space, SSD or HDD|256060514304<br />126.63 GB<br />SSD|Get-PhysicalDisk \| Format-List \*<br />Get-Volume|
|Partition style of the system disk|GPT|Get-Disk|
|OS and version|Майкрософт Windows 11 Домашняя<br />10.0.22631 Н/Д построение 22631|systeminfo|



## 1b. A machine you have never seen: a thin-and-light laptop

1. No, this laptop would not be able to run both VMs at the same time. With 8GB RAM available the system would already use some of it in idle. Ubuntu needs 2GB, Windows needs 4GB minimum. This leaves 2GB RAM for the host, which is already a tight fit. The laptop has 31GB storage free, and windows needs 64GB minimum as the installer refuses anything below these. Not to mention that Ubuntu would also need 20-25GB for comfortable work. CPU allocation, on the contrary works for both, the main issue is the storage.
2. The best solution would be to increase the storage, buying an SSD. Although on this machine in particular it cannot be upgraded, as disk (and ram) is soldered to the motherboard. It cannot be upgraded in this case. Instead, one solution would be to buy a new laptop (16GB RAM, at least 512GB storage for starters), which does not have it's components soldered, so that it can be upgraded in the future. Second solution is to clear disk space, by saving part of it in the cloud, leaving space for both VMs.

