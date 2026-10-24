# Lab N - 02



Machine: DESKTOP-EA7P1RT, lab01, lab02

Date: 24.10.2026



## What i did



1. Install Windows 11 Enterprise evaluation ISO
2. Set up lab02 VM (64GB disk, 4GB RAM)
3. Installed Win11 and the guest tools
4. Assigned "evidence" as shared folder
5. Chose VMware NAT as the network
6. ipconfig / ip addr - found wach guests' IPv4 address
7. Couldn't save the evidence via CMD. Was able to access the path via PowerShell
8. cd \\\\vmware-host\\Shared Folders\\evidence
9. ipconfig > ipconfig-windows.txt
10. ssh arrrs@192.168.201.129 ip addr > ip-addr-linux.txt
11. git add . / git commit -n "creating a file and testing smt" / git push
12. Tested whether PowerShell pushed files looked broken on GitHube (they did not)
13. ping -n 192.168.201.129
14. ping -c 4 192.168.201.130
15. Get-NetConnectionProfile
16. New-NetFirewallRule -DisplayName "Lab02 - allow ping (ICMPv4 echo)" -Direction Inbound -Protocol ICMPv4 -IcmpType 8 -RemoteAddress LocalSubnet -Action Allow
17. Get-NetFirewallRule -DisplayName "Lab02\*"
18. ping -n 4 192.168.201.129 > ping-both-ways.txt
19. ssh student@192.168.201.129 ping -c 4 192.168.201.130 >> ping-both-ways.txt
20. ping -n 2 8.8.8.8 / ping -c 2 8.8.8.8
21. Took a host-memory-before.png screenshot from task manager
22. Allocated 4GB for Ubuntu and 12GB for Windows 11
23. Windows 11 VM refused to start, giving an "Not enough system resources..." error
24. Increased host's paging size to 16-24GB
25. sudo apt install -y stress-ng / stress-ng --vm 1 --vm-bytes 90% --vm-keep --timeout 300s
26. Loaded multiple Youtube and reddit pages on Windows 11 VM
27. Took a host-memory-after.png screenshot from task manager
28. Changed the VMs' values back to normal



## Result

Why this network mode:

I used VMware NAT mode for both guests.

Requirement: the guests must reach each other and the internet.

VMware NAT meets both because it puts both guests on the same virtual network and handles routing to the internet.

I did not use bridged because it can sometimes fail depending on how home routers handle local virtual adapters.

I did not use host-only because it isolates the network completely and blocks all internet access.



Network diagram:

&#x09;		Internet

&#x09;		|

&#x09;\[ Host: my PC ]   real network: Wi-Fi, 192.168.1.11

&#x09;		|

&#x09;( NAT router of the hypervisor )   gateway 192.168.201.2

&#x20;                    |

&#x09;===== virtual switch: VMnet8 (NAT) ======

&#x09;	|			|

&#x09;\[ lab01-ubuntu ]          \[ lab02-win11 ]

&#x09;ens33                     Ethernet0

&#x09;192.168.201.129           192.168.201.130



The firewall:

The first ping from Linux to Windows failed because Windows classifies the new network as Public and blocks incoming echo requests by default. To fix this, I added an inbound rule to allow only ICMPv4 type 8 echo requests from the local subnet: New-NetFirewallRule -DisplayName "Lab02 - allow ping (ICMPv4 echo)" -Direction Inbound -Protocol ICMPv4 -IcmpType 8 -RemoteAddress LocalSubnet -Action Allow



Overcommitment experiment:

Host total - 16GB (13.9 usable)

Sum promised to the guests - 20GB (8GB Ubuntu, 12GB Windows 11)

Before - 6.3GB in use, 7.6GB available, 11.9/16.9GB committed

After - 13.6GB in use, 317MB available, 33.5/33.9GB committed

How host felt - slight mouse lag, huge delay in opening/closing/moving windows

What the host and hypervisor did - since memory was dropping close to zero, the host heavily relied on memory compression and page file swapping. The hypervisor honored the memory requests, causing system slowdowns and high disk usage as the host and guests competed for physical memory.



## What did not work the first time:

Could not access the shared evidence folder from cmd. Path or permission issues when accessing shared folders directly via standard cmd. Tested in PowerShell and verified file transfer and GitHub push without issues.

The initial ping command did not work. The Windows Defender Firewall was blocking incoming ICMP echo requests by default. Opened PowerShell as Administrator.

The Windows 11 VM refused to start during the overcommitment task, giving an "Not enough system resources..." error. The host physical RAM and virtual page file limits were fully exhausted, preventing the hypervisor from allocating the requested memory pool. Increased the host's paging file size to 16–24 GB.





### Evidence

evidence/ipconfig-windows.txt - output of ipconfig inside the Windows guest showing its IPv4 address on VMnet8

evidence/ip-addr-linux.txt - output of ip addr inside the Ubuntu guest showing its network configuration and IP address

evidence/ping-both-ways.txt - results of ping tests showing successful communication in both directions between the guests

evidence/host-memory-before.png - host Task Manager memory performance view before overcommitment

evidence/host-memory-after.png - host Task Manager memory performance view under heavy memory pressure

