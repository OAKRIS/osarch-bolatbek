# Build guide: a safe test computer (virtual machine)

Who this is for: anyone who can install a program and follow instructions. No technical knowledge is needed.

What you will get: two "computers inside your computer", one Linux and one Windows. The developer can break them freely, and you can reset them in one click.

Time: about 2 hours, mostly waiting for installers.



### Words you will meet:

* Host: the real PC you are working on.
* Guest / VM: a pretend computer running inside the host.
* ISO: a file that works like an installation disc.
* Snapshot: a saved copy of the VM's state that you can return to.
* NAT: Network Address Translation — the guest reaches the internet through the host's address.



### Before you start

You need a Windows PC (the host) with at least 16 GB of memory and 120 GB of free disk space, an internet connection, and these downloads:

* VMware Workstation Pro (free for personal use) from the Broadcom site. You need a Broadcom account to download it.
* Ubuntu Server LTS ISO (the current LTS version) from ubuntu.com/download/server.
* Windows 11 Enterprise evaluation ISO from microsoft.com/software-download/windows11.
* Licence: Windows runs in evaluation mode without a key. For use beyond testing, ask the office for a licence.



#### Step 1. Check that your PC can run virtual machines

Press Ctrl+Shift+Esc → Performance → CPU. Look for "Virtualization: Enabled".

Why: if it says Disabled, VMs will not start. Enable it in the PC's BIOS (search "enable virtualization" plus your PC model).



#### Step 2. Install VMware Workstation Pro

Run the installer and accept the defaults. Restart if asked.

Why VMware: it is free for personal use and the teaching labs use it. Any other hypervisor would need different steps.



#### Step 3. Plan the resources before you build

Defaults are somebody else's guess about somebody else's computer. Start from your host's totals, subtract what the host needs, and give the guests what they need plus room for the second one.

|Resource|Host total|Host in use|Ubuntu guest|Windows guest|Total given to guests|
|-|-|-|-|-|-|
|Memory|14936838144 bytes (\~13.9 GB)|\~2 GB|2 GB|4 GB|6 GB|
|Logical CPUs|4|4|2|2|4|
|Disk free|40.3 GB|-|20 GB|64 GB|84 GB (limits not pre-allocated)|



#### Step 4. Create the Ubuntu VM

In VMware: File → New Virtual Machine → Typical → Installer disc image file (iso) → choose the Ubuntu Server ISO. Name the VM lab01-ubuntu.

https://evidence/01-new-vm-wizard.png

Why Typical: the wizard fills in safe defaults and installs Ubuntu automatically. Custom mode would only add choices you do not need.

Why Ubuntu Server, not Desktop: there is no graphical shell, so there is nowhere to hide from the console. The rest of the course assumes a console guest.



#### Step 5. Give it memory and processors

VM → Settings → Hardware. Set Memory = 2 GB and Processors = 2.

Why these numbers: the Windows guest needs its 4 GB minimum, so Ubuntu gets 2 GB to leave the host usable. Together the two guests use about half of the host's memory.



#### Step 6. Set the disk size

In the wizard's disk page, set Maximum disk size = 20 GB. Leave Allocate all disk space now unticked.

https://evidence/02-disk-settings.png

Why: the disk file grows only as the VM fills it, so 20 GB is a limit, not an immediate cost. Ticking the box would take all 20 GB from the host straight away. The trade-off is slightly slower disk growth.



#### Step 7. Choose how the VM connects to the network

VM → Settings → Network Adapter → NAT.

https://evidence/03-network-adapter.png

Why NAT and not Bridged: NAT lets the VM use the host's internet connection and keeps it hidden from the office network. Bridged gives the VM its own network identity, which a filtered office network may refuse. Use Bridged only if another computer must reach the VM.

Check: the VM can open a website after step 8.



#### Step 8. Install Ubuntu Server

Click Finish, then Power on. The installer is a text menu: arrow keys move, Space ticks a box, Enter confirms. The install takes 10–20 minutes. Log in with the username and password from the wizard.

Why wait: do not close the window while it installs, or the VM must be rebuilt.



#### Step 9. Check that Linux works

Open a terminal and type these two commands, pressing Enter after each:

*uname -a*

*ip addr*

Why: uname -a proves the system is running Linux. ip addr should show an address like 192.168.x.x, which proves the VM is on the network.



#### Step 10. Create the Windows VM

File → New Virtual Machine → Typical → choose the Windows 11 ISO. Choose Windows 11 x64. Name it lab02-win11. Set Memory = 4 GB, Processors = 2, Disk = 64 GB (not pre-allocated), Network = NAT, as in steps 5–7.

https://evidence/01-new-vm-wizard.png

Why these numbers: Windows 11 refuses to install with less than 4 GB of memory or 64 GB of disk. When asked for an encryption password (it protects the VM's virtual security chip, the TPM), write it down.



If the installer says "This PC can't run Windows 11", check step 10: memory ≥ 4 GB, disk ≥ 64 GB, and the guest type is Windows 11 (not Windows 10). Do not use registry tricks to skip the checks — set the VM up properly.



#### Step 11. Install Windows

Power on and follow the installer. If it asks for a product key, choose I don't have a product key. Choose Windows 11 Pro and Custom install (not Upgrade).

Why no key: Windows works without one for testing. Skipping it avoids using a license on a throwaway machine.

Why a local account: when asked to sign in, choose Sign-in options → Domain join instead to create a local account. Do not use a personal Microsoft account in a lab VM.



#### Step 12. Test the internet and the connection between the two VMs

In each VM, open a web browser and load any website.

Find each VM's address. Linux: ip addr. Windows: open Command Prompt and type ipconfig.



From Linux, type ping -c 4 <Windows address>. From Windows, type ping <Linux address>.

Save the Linux ping result in evidence/05-ping-between-guests.txt.

Why: "4 packets received, 0% loss" means the two machines can see each other. If Linux cannot ping Windows, Windows Defender Firewall is probably blocking it: in Windows, search Windows Defender Firewall with Advanced Security → Inbound Rules, and enable File and Printer Sharing (Echo Request – ICMPv4-In). This is what happened in my first attempt: Ubuntu could not ping Windows until I enabled that rule.



Turning the firewall off makes ping work and scores zero. Allow ICMP specifically; never switch the whole firewall off.



#### Step 13. Take a snapshot (the "undo button")

With each VM selected, choose VM → Snapshot → Take Snapshot. Name it clean-install.

https://evidence/06-snapshot-list.png

Why: if the developer breaks the machine, VM → Snapshot → Revert returns it to this exact state in seconds. This is what "try things without breaking anything" means in practice.



#### Step 14. Making the next machine (the quick way)

Shut down the VM, then VM → Manage → Clone → Full clone. The copy is a new, identical computer.

Why: this takes about 5 minutes against 2 hours for steps 4–13, so repeat this guide only if no clone exists.



#### If something goes wrong:



|Problem|Fix|
|-|-|
|VM won't start, mentions virtualization|Repeat step 1 and enable it in BIOS.|
|Windows installer says "This PC can't run Windows 11"|Check step 10: memory ≥ 4 GB, disk ≥ 64 GB.|
|No internet in the VM|Step 7: confirm the adapter is NAT and "Connected" is ticked.|
|Host becomes very slow|Close the VMs and reduce their memory (steps 5 and 10).|
|Linux cannot ping Windows|Enable File and Printer Sharing (Echo Request – ICMPv4-In) in Windows Defender Firewall.|
|Windows asks for a product key|You have the wrong ISO — stop and ask; the evaluation image does not ask.|
|Storage shows less space than the disk created|Normal with LVM: the installer leaves part of the disk unused on purpose.|



