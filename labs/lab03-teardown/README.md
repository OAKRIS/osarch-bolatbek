# Lab N ─ 03



Machine: DESKTOP-EA7P1RT, lab01

Date: 30.10.2026



## What i did:



1. Get-CimInstance Win32\_ComputerSystem | Format-List \*
2. Found the manufacturer, model and system family
3. Found the service manual
4. Opened my PC and took a photo
5. Marked the components on the photo
6. Filled the table in README.md
7. Filled the upgrade verdict part
8. Get-CimInstance Win32\_Processor | Format-List \* / Get-CimInstance Win32\_ComputerSystem | Format-List \* / Get-CimInstance Win32\_PhysicalMemory | Format-List \* / Get-PhysicalDisk | Format-List \* / Get-CimInstance Win32\_BaseBoard | Format-List \*
9. lscpu / free -h / sudo dmidecode -t memory / lsblk -d -o NAME,MODEL,SIZE,ROTA,TRAN / sudo dmidecode -t baseboard
10. Filled the OS table
11. Start-Transcript -Path windows-specs.txt / -five commands- / Stop-Transcript
12. lscpu > linux-specs.txt / free -h >> linux-specs.txt / sudo dmidecode -t memory >> linux-specs.txt / lsblk -d -o NAME,MODEL,SIZE,ROTA,TRAN >> linux-specs.txt / sudo dmidecode -t baseboard >> linux-specs.txt
13. scp arrrs@192.168.201.129:\~/linux-specs.txt "C:\\Users\\Admin\\OneDrive\\Desktop\\lol\\osarch-bolatbek\\labs\\lab03-teardown\\evidence\\"



## Result:



1. The machine:

&#x09;Model:

&#x09;	Manufacturer                : HP

&#x09;	Model                       : HP Desktop M01-D0xxx

&#x09;	SystemFamily                : 103C\_53311M HP Desktop PC

&#x09;How i found it: Get-CimInstance Win32\_ComputerSystem | Format-List \*

&#x09;Link to the manual: https://kaas.hpcloud.hp.com/pdf-public/pdf\_3941897\_en-US-1.pdf

https://support.hp.com/gb-en/document/c06429270

2\. Components:

|Annotated figures|What|Connects to|Limits|
|-|-|-|-|
|1|CPU socket AMD Socket AM4|AMD Ryzen processor; connects the CPU to the motherboard and its chipset/memory system|Supports compatible AM4 processors with a maximum TDP of 65 W. HP lists specific supported Picasso and Matisse processors.|
|2|Two 288-pin DDR4 UDIMM memory slots|RAM modules; connect to the CPU's integrated memory controller|Maximum 32 GB total (2 × 16 GB), unbuffered DDR4 memory.|
|3|PCI Express x16 expansion slot|Dedicated graphics card or another compatible PCIe expansion card|Max bus bandwidth capability of 16 lanes|
|4|M.2 Socket 1, Key A|Wireless LAN expansion module|Fits M.2 2230 size network modules only|
|5|SATA Data Ports Two Serial ATA 3.0 Ports|SATA storage devices, such as HDDs or SATA SSDs|Maximum bandwidth restriction of 6.0 Gbps|
|6|CR2032 CMOS/RTC battery|Motherboard's real-time clock and firmware settings|3V output power capacity (non rechargeable)|
|7|System power supply (PSU) and motherboard power connector|Motherboard proprietary power rails|Uses custom HP small-form-factor electrical pinouts|

3\. Upgrade verdict:

|Component|Manual says|OS says|Verdict|
|-|-|-|-|
|Memory|The HP Erica motherboard has two 288-pin DDR4 UDIMM slots and supports up to 32 GB of unbuffered memory (2 × 16 GB).|26 (DDR4), speed 2667 MHz, and total physical memory size of 14936838144 bytes.|Yes. The memory is upgradeable because the motherboard has two replaceable UDIMM slots. It supports up to 32 GB.|
|Storage|The motherboard has an M.2 Socket 3, Key M connection for an SSD using PCIe x2.|MediaType = SSD and BusType = NVMe|Yes. The existing NVMe SSD can be replaced with a compatible M.2 NVMe drive.|
|CPU|The motherboard uses an AMD Socket AM4 and supports compatible processors with a maximum TDP of 65 W. HP lists specific supported processors, including Ryzen 5 3400G, Ryzen 5 3500 and Ryzen 7 3700X.|AMD Ryzen 3 3200G with Radeon Vega Graphics, with 4 cores and 4 logical processors.|Yes. The CPU is socketed and can be replaced. However, the replacement must be one of the processors supported by HP for this motherboard and must meet its power requirements.|



4\. Three(two)-OS table:

|Fact|Windows|Linux (VM)|
|-|-|-|
|CPU model|Get-CimInstance Win32\_Processor \| Format-List \*<br />Name ─ AMD Ryzen 3 3200G with Radeon Vega Graphics|lscpu<br />Model name ─ AMD Ryzen 3 3200G with Radeon Vega Graphics|
|Cores|Get-CimInstance Win32\_Processor \| Format-List \*<br />NumberOfCores ─ 4|lscpu<br />Core(s) per socket ─ 1<br />Socket(s) ─ 2|
|Logical CPUs|Get-CimInstance Win32\_Processor \| Format-List \*<br />NumberOfLogicalProcessors ─ 4|lscpu<br />CPU(s) ─ 2|
|Max clock|Get-CimInstance Win32\_Processor \| Format-List \*<br />MaxClockSpeed ─ 3600|CPU max MHz ─ N/A (VM)|
|Cache|Get-CimInstance Win32\_Processor \| Format-List \*<br />L2CacheSize ─ 2048<br />L3CacheSize ─ 4096|lscpu<br />L1d ─ 64 KiB (2 instances)<br />L1i ─ 128 KiB (2 instances)<br />L2 ─ 1 MiB (2 instances)<br />L3 ─ 8 MiB (2 instances)|
|Total memory|Get-CimInstance Win32\_ComputerSystem \| Format-List \*<br />TotalPhysicalMemory ─ 14936838144|free -h<br />total ─ 1.6Gi|
|Memory type/speed|Get-CimInstance Win32\_PhysicalMemory \| Format-List \*<br />SMBIOSMemoryType ─ 26 (DDR4)<br />Speed ─ 2667|dmidecode -t memory<br />Type ─ EDO DIMM / DIMM<br />Speed ─ Unknown|
|Disk type|Get-PhysicalDisk \| Format-List \*<br />MediaType ─ SSD|lsblk -d -o NAME,MODEL,SIZE,ROTA,TRAN<br />ROTA ─ 1 (HDD)|
|Disk interface|et-PhysicalDisk \| Format-List \*<br />BusType ─ NVMe|lsblk -d -o NAME,MODEL,SIZE,ROTA,TRAN<br />TRAN ─ -/spi/sata|
|Motherboard|Get-CimInstance Win32\_BaseBoard \| Format-List \*<br />Manufacturer ─ HP<br />Product ─ 8643|sudo dmidecode -t baseboard<br />Manufacturer ─ Intel Corporation<br />Product Name ─ 440BX Desktop Reference Platform|







## What did  not work the first time:



Photographed only the motherboard instead of the whole open PC; realized the mistake after closing the case; reopened it and retook the complete photo.

Saw an error that VMs were already in use with a prompt to delete them; turned out a hidden VMware instance was running in Task Manager; ended the lingering tasks to restore ownership safely without deleting files.



## Evidence:



evidence/photos ─ photographs of physical components

evidence/windows-specs.txt ─ CPU, memory, and disk specifications read from the Windows command line

evidence/linux-specs.txt ─CPU, memory, and disk specifications read from the Linux command line

