# Lab N - 01



Machine: DESKTOP-EA7P1RT, lab01

Date: 17.10.2026



## What i did



1. Install VMware
2. Get Ubuntu server
3. Sized what parameters i need for my VM
4. Created VM
5. Took a screenshot of its settings for evidence
6. Installed Ubuntu
7. Logged in
8. lsb-release -a
9. nproc
10. free -h
11. df -h /
12. ip a
13. In host terminal, cd C:\\Users\\Admin\\OneDrive\\Desktop\\lol\\osarch-bolatbek\\labs
14. mkdir lab01-first-vm
15. cd lab01-first-vm
16. mkdir evidence
17. New-Item README.md -ItemType File
18. git add . / git commit -m "created lab01 folder" / git push
19. cd evidence
20. ssh arrrs@192.168.201.129 lsb\_release -a > guest-release.txt
21. Shut down guest and make a snapshot
22. Powered on and deleted the hostname
23. Took a screenshot of the broken state for evidence
24. Reverted back
25. cat /etc/hostname
26. Took a screenshot of the snapshot list for evidence
27. Added screenshots into evidence folder
28. git add . / git commit -m "filled evidence folder" / git push
29. Deleted desktop.ini file from GitHub
30. git config pull.rebase false / git pull origin main --no-edit / git push origin main



## Result

My host has 4 threads, 16 GB of RAM and 49.45 GB free on the VM volume (Lab 0).

Idle, the host already uses about 7.9 GB of RAM.

I gave the guest 2 vCPU of my 4 threads because it balances performance by leaving 2 threads free for the host OS.

I gave it 2 GB of my 16 GB RAM because Ubuntu Server needs minimal overhead to run the console, and I keep 2-3 GB for the Windows guest in Lab 2.

I gave it a 20 GB dynamically allocated disk of my 49.45 GB free because it provides plenty of room for software packages while leaving over 25 GB for the next lab's Windows VM.



## What did  not work the first time:

After pushing evidence files GitHub had desktop.ini file alongside them. After deleting it from GitHub i couldn't push commits. Local repo and GitHub were out of sync, which blocked pushing. I pulled and merged commits, then pushed them.



## Evidence

evidence/vm-settings.png - CPU, memory, and disk which have measurements that i gave it

evidence/guest-release.txt - output of lsb\_release -a inside the guest showing Ubuntu Server LTS

evidence/broken.png - the guest after deleting the hostname

evidence/snapshot-list.png - snapshot list showing clean-install after the successful rollback

