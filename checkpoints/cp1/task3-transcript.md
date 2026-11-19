# Task 3: Transcript (office-srv, evidence pack of Mon 2026-11-16 ~09:42 UTC)

| # | Evidence file (command) | What I expected | What I found | What I concluded |
|---|-------------------------|-----------------|--------------|------------------|
| 1 | 00-README.txt | list of files, the command behind each | 22 files, collected over SSH as aigerim | I work from files, not the live server. |
| 2 | 01 (hostnamectl), 02 (uptime) | what kind of machine, how long up | Ubuntu 24.04.1 VM on VirtualBox, up 6d 23h, Boot ID 9b2d61f0... | Last boot was about Nov 9 09:50. No reboot since. |
| 3 | 05 (systemctl --failed) | nightly-report failing | two failed units: legacy-sync and nightly-report | Two leads. Only nightly-report is named in the ticket. |
| 4 | 06 (systemctl list-timers) | timer dead or never scheduled | nightly-report.timer is active: last run 09:42:01, next 09:44:01 | Timer fires every 2 minutes. Dead end: timer ruled out. |
| 5 | 08 (systemctl cat service + timer) | typo or wrong path in ExecStart | ExecStart=/usr/local/bin/nightly-report.sh, Type=oneshot | Unit files look correct. Next check: why does that path fail? |
| 6 | 07 (systemctl status nightly-report) | an exit code and a reason | status=203/EXEC, "Failed to execute ...: No such file or directory" | systemd could not start the program at all. |
| 7 | 13 (ls -l /usr/local/bin) | empty directory if the file is missing | file exists: -rwxr-xr-x root, 91 bytes, modified Nov 9 14:20 | File exists and has correct permissions. |
| 8 | 09 (journalctl -u nightly-report) | when failures began | success every 2 min through 14:18:01 on Nov 9, first failure 14:20:01 | Broke between 14:18 and 14:20 on Nov 9. |
| 9 | 14 (ls -l /var/log), 15 (tail nightly-report.log) | latest log entries | last entry Nov 9 14:18:01, log mtime 14:18 | Log stopped because the script stopped. |
| 10 | 03 (df -h), 04 (free -h) | disk full or no memory | / 48% used, 5.8 G free; RAM 1.5 Gi available | Dead end: resources ruled out. |
| 11 | 10 (status legacy-sync), 11 (journalctl legacy-sync) | a related failure | ExecStart=/bin/false, "no longer used", fails once per boot | Dead end: unrelated. |
| 12 | 21 (apt history) | something that changed | Nov 9 09:38-09:41 full-upgrade: bash, systemd, kernel, openssh | Ruled out as the cause. |
| 13 | 12 (journalctl -p err -b) | other errors explaining it | vboxguest/vmwgfx lines at boot, legacy-sync, nightly-report | Nothing new. |
| 14 | 22 (bash history, aigerim) | what was done on Nov 9 | after reboot: `sudo cp ~/nightly-report.sh /usr/local/bin/...`, `chmod +x`, start, status, `ls -l`, `chmod 755`, start, status, exit | The script was replaced by a copy from the home directory. |
| 15 | 16 (cat nightly-report.sh) | syntax error or wrong command | three correct-looking lines, `#!/bin/bash`, appends date, `df -h /`, `free -h` to the log | The text is fine by eye. |
| 16 | 19 (run by hand) | runs, or a syntax error | `cannot execute: required file not found` | Same error outside systemd. |
| 17 | 20 (ls -l /bin/bash) | /bin/bash missing | exists, rwxr-xr-x, 1446024 bytes | The interpreter exists. |
| 18 | 17 (cat -A) | hidden characters | every line ends in `^M$`, including `#!/bin/bash^M$` | The file has Windows (CRLF) line endings. |
| 19 | 18 (head -1 ... \| od -c) | confirm line 1 | `# ! / b i n / b a s h \r \n`, 13 bytes | The kernel looks for `/bin/bash\r`, which does not exist. |

## Root cause

`/usr/local/bin/nightly-report.sh` was replaced on Nov 9 with a copy that has Windows CRLF line endings. The shebang line therefore names `/bin/bash\r`, which does not exist, causing exec to fail with status 203/EXEC.

## The fix

1. Preserve the broken file: `sudo cp -p /usr/local/bin/nightly-report.sh /root/nightly-report.sh.crlf`
2. Convert CRLF to LF: `sudo sed -i 's/\r$//' /usr/local/bin/nightly-report.sh`
3. Fix the source copy: `sed -i 's/\r$//' ~/nightly-report.sh`
4. Verify with `file /usr/local/bin/nightly-report.sh` and `stat -c '%s bytes, mode %a, owner %U' /usr/local/bin/nightly-report.sh`
