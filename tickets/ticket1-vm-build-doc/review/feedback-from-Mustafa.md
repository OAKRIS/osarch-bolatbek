# Feedback on Amira Bolatbek‘s BUILD.md



Reviewer: Nuraiym Mustafa

Commit reviewed: https://github.com/OAKRIS/osarch-bolatbek/blob/ae6cc1aee2ac59bc9b18b5e58042155a19ff1087/tickets/ticket1-vm-build-doc/BUILD.mdHypervisor I used: VMware Workstation



# First step where I got stuck



Step 3. This step tells the reader to check the host’s available resources and use them to work out how much to give the Ubuntu and Windows VMs.



What I did: I tried to figure out how to find the host’s total memory, how much of it was already in use, the number of logical CPUs, and the free disk space, so I could decide what to assign to each VM.



Where I had to guess: The guide gives specific numbers but never says where to find them or how they were calculated. Someone new to this wouldn’t know where to look on their own machine, and they couldn’t tell whether the suggested split would work for their hardware.

# 

# Other places I hesitated



Steps 4 and 10: Both point to 01-new-vm-wizard.png. I couldn’t tell whether the screenshot shows the Ubuntu VM, the Windows VM, or both.

Steps 4-7: The order jumped around. The guide goes from the VM creation wizard to the VM settings, then back to the disk page of the wizard, which made it hard to follow.

Step 12: It says how to save the Linux ping result, but not the Windows one. It also doesn’t say how to keep proof that both guests can reach the internet.

Step 14: It explains how to clone a VM, but not whether the clone needs a new name or any network changes afterward.



# Settings I wouldn’t have chosen without the document



NAT instead of Bridged networking. The explanation about internet access and keeping the VMs separate from the office network made the choice make sense to me.

A dynamically allocated virtual disk instead of pre-allocating the full size. The guide made it clear that the disk has a maximum size but doesn’t take up all that space on the host right away.



# One thing that worked well



The guide explains why you’re making each important choice, rather than just listing what to click. The sections on NAT networking, snapshots, and the Windows firewall were especially helpful, and they make the guide much easier to follow for someone new to virtualization.

