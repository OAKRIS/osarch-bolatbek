# Lab N ─ 06



Machine: DESKTOP-EA7P1RT, lab01, lab02

Date: 20.11.2026



## What i did:



1. Powered on win11 VM
2. Downloaded process explorer on Win11 VM
3. Opened notepad.exe
4. Took a screenshot with Process explorer and Task manager
5. Powered off win11 VM and powered on Ubuntu VM
6. nano test.txt \&
7. htop
8. ps -o pid,ppid,stat,cmd -p 2402
9. ls -l /proc/2402/fd | wc -l
10. Took a screenshot with htop
11. Powered off Ubuntu VM and took a snapshot
12. Powered on Ubuntu VM
13. ls
14. cd /labs
15. mkdir evidence6
16. cd evidence6
17. echo "Before:" > memory-pressure.txt
18. free -h >> memory-pressure.txt
19. swapon --show >> memory-pressure.txt
20. Opened a second terminal
21. vmstat 1
22. python3 -c "
import time
a = \[]
while True:
    a.append(b'x' \* 100\_000\_000)
    print(len(a) \* 100, 'MB', flush=True)
    time.sleep(1)
"
23. echo "During:" >> memory-pressure.txt
24. free -h >> memory-pressure.txt
25. After the process is killed: echo "After:" >> memory-pressure.txt
26. free -h >> memory-pressure.txt
27. sudo dmesg -T | grep -i oom > \~/labs/lab06-processes/evidence/kernel-log-extract.txt
28. scp arrrs@192.168.254.130:/home/arrrs/labs/evidence6/kernel-log-extract.txt arrrs@192.168.254.130:/home/arrrs/labs/evidence6/memory-pressure.txt "C:\\Users\\bolat\\OneDrive\\Desktop\\lol\\osarch-bolatbek\\labs\\lab06-processes\\evidence"



## Result:



### Three-tool comparison:

A text editor (notepad.exe and nano) on each OS was chosen, as one process cannot run in both guests.

||Task manager|Process explorer|htop|
|-|-|-|-|
|process|notepad.exe|notepad.exe|nano|
|PID|9428|9428|2134|
|parent|-|5656|1996|
|in physical RAM|Working set 104,436 K|Working Set 106,484 K|RES 3,708 K|
|shared|-|WS Shareable 73,408 K|SHR 2,796 K|
|private memory|Commit size 43,404 K<br />Memory column 25,900 K|Private Bytes 43,584 K|not a default column|
|handles|972|991|3|
|virtual size|-|2,152,170,864 K|VIRT 7,144 K|



### Where they disagree:

* Working set (Task Manager), Working Set (Process Explorer) and RES (htop) all just mean RAM currently in use. Commit size and Private Bytes are also the same thing under different names.
* Task Manager’s main Memory column is smaller (\~25 MB vs \~104 MB) because it only shows private RAM, while Working Set also includes shared stuff like DLLs.
* Virtual size looks massive (\~2 TB) because Windows reserves lots of address space that isn’t actually in RAM yet. Working Set is what’s really in physical memory.
* Windows handles track things like registry keys, threads and events, while Linux file descriptors are mostly open files/pipes/sockets, so 991 and 3 aren’t direct equivalents.
* Numbers like 972 and 991 handles differ slightly because the screenshots weren't taken at the exact same second (a couple of minutes apart).



## What did  not work the first time:

Initial process chosen for this lab was SSH. Win11 VM had no issues, but searching on Linux only showed "sshd" daemon entries rather than an interactive "ssh" client process. It turned out a backgrounded "ssh" client would not stay open reliably in my terminal session, unlike a "systemd"-managed service. I switched to using notepad on Windows and nano on Linux so I could inspect active processes consistently and finish the comparison.



## Evidence:

evidence/task-manager.png ─ Task manager screenshot of the process (notepad.exe)

evidence/process-explorer.png ─ Process explorer screenshot of the process (notepad.exe)

evidence/htop.png ─ htop screenshot of the process (nano)

evidence/memory-pressure.txt ─ free -h output before, during, and after the memory test

evidence/kernel-log-extract.txt ─ dmesg extract showing the OOM kill

