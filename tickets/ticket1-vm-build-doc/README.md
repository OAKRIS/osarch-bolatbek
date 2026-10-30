# Ticket N1 — "New developer starts Monday, needs a test machine"



1. ## What was asked

From: Aigerim T. (Office Manager)

Subject: test machine for the new dev



Hi, we have someone starting on Monday and he needs a test computer

set up. Nothing special, just so he can try things without breaking

anything. Can you write down how to do it so I can do the next one

myself? Thanks



## 2\. What was actually needed

|The request says|What it leaves open|What i assumed, why|
|-|-|-|
|"a test computer"|Physical PC or virtual machine?|Virtual machines on the existing host PC. No hardware purchase is mentioned, and a VM can be reset in seconds with a snapshot.|
|"Nothing special"|Which operating system? What does the developer work on?|Both Linux (Ubuntu) and Windows. Most development targets one or the other, and not knowing which, both are cheap to provide.|
|""without breaking anything""|Break what: the test machine, the developer's own PC, or the company network?|All three. The VMs are isolated from the host, use NAT so they are not exposed on the office network, and each has a snapshot so damage can be undone in one click.|
|"so I can do the next one myself"|Who reads the document?|Aigerim, the Office Manager, who is not technical. So every step names the exact button, explains unfamiliar words, and ends with a check of what she should see.|
|"on Monday"|How much capacity does the host have? Is there a Windows licence?|I measured the host myself (section 3). Windows runs in evaluation mode without a key; a licence is needed for long-term use and I flagged that in BUILD.md.|

Where the request is slightly wrong: Aigerim asks for a document, but her real goal is a repeatable result. A document alone still takes an hour per machine. So BUILD.md gets the work done now, and a clone of the finished VM lets the next machine be made in minutes.



What I would have asked her:

Which operating system and tools does the developer need?

Is there a Windows licence, or should an evaluation copy be used?

Which PC will host the VM, and how much free memory and disk does it have?

Does the VM need to be reached from other computers? That would change NAT to Bridged.

Measured host figures



|Item|Value|Command/source|
|-|-|-|
|Host total memory|14936838144 (\~13.9 GB)|Get-CimInstance Win32\_ComputerSystem → TotalPhysicalMemory|
|Host logical CPUs|4|Get-CimInstance Win32\_Processor \| Format-List \* → NumberOfLogicalProcessors|
|Host free disk|40.3GB|File Explorer → This PC|



Resource allocation. Rule used: the guests together take at most half of host memory and a quarter of the logical CPUs, so the host stays responsive.

||Ubuntu guest|Windows guest|Total|Share of host|
|-|-|-|-|-|
|Memory|2GB|4GB|6GB|\~10 GB of \~14 GB available to guests|
|CPUs|2|2|4|all 4 logical CPUs, split between the guests|
|Disk|20GB|64GB|84GB|40.3 GB free (limits are not pre-allocated)|



Settings that differ from default:

|Setting|Default|Mine|Why|
|-|-|-|-|
|Memory (Ubuntu)|4 GB|2 GB|The Windows guest needs its 4 GB minimum; giving Ubuntu 2 GB keeps the host responsive.|
|Disk size (Ubuntu)|20 GB|20 GB|Kept the default — enough for a test machine.|
|CPUs|2|2|Kept the default — enough for a test machine.|
|Memory/disk (Windows)|4 GB / 64 GB|4 GB / 64 GB|Minimum for Windows 11.|
|Disk allocation|Not pre-allocated|Not pre-allocated|Pre-allocating takes the full space at once. I rejected it to save host disk; the trade-off is slightly slower disk growth.|
|Network|NAT|NAT|Bridged was rejected because the teaching network filters by MAC address|



## 3\. What I did



1. Measured the host's memory, CPUs and free disk with PowerShell
2. Sized the guests from those figures.
3. Confirmed virtualization is enabled in Task Manager
4. Installed VMware Workstation with default options.
5. Created the Ubuntu VM with the Typical wizard: 4 GB memory, 2 CPUs, 40 GB disk, not pre-allocated.
6. Kept the network adapter on NAT. Bridged was rejected because the teaching network filters by MAC address.
7. Installed Ubuntu and checked it with uname -a and ip addr.
8. Created and installed the Windows 11 VM: 4 GB, 2 CPUs, 64 GB disk, NAT, installed without a product key.
9. Tested internet access and ping between the guests.
10. Took a snapshot named clean-install on both VMs.
11. Wrote BUILD.md from these steps, with a screenshot and a reason for each, and added a clone step for faster repeats.



One thing that went wrong: the first time I tried ping from Ubuntu to Windows, every packet was lost. I checked the IP with ipconfig on Windows (it was 192.168.201.130) and the Ubuntu side could reach it, but Windows was blocking the request. I fixed it by enabling File and Printer Sharing (Echo Request – ICMPv4-In) in Windows Defender Firewall with Advanced Security. After that the ping test in evidence/05-ping-between-guests.txt shows 0% loss.



## 4\. Evidence

evidence/01-new-vm-wizard.png — The VM was created through the wizard with the stated OS and name.

evidence/02-disk-settings.png — Disk size and non-pre-allocated setting.

evidence/03-network-adapter.png — The network adapter is set to NAT on both VMs.

evidence/04-guest-uname.txt — The Linux guest is installed and on the network (uname -a, ip addr → 192.168.201.129).

evidence/05-ping-between-guests.txt — Ubuntu reaches the internet (8.8.8.8, 0% loss) and the Windows guest (192.168.201.130, 0% loss).

evidence/06-snapshot-list.png — A clean-state snapshot exists on both VMs, so changes can be undone.



## 5\. What I would do differently

I would build the Ubuntu guest once, export it as a template, and clone it for the next machine. Following my own 13 steps took about 90 minutes, while a clone takes about two. I would also record the Windows activation question with Aigerim before Monday rather than assuming an evaluation copy is acceptable.

