# Task 3 - Diagnosis

| # | File / Command | What I checked for | What I found | What it means |
|---|---|---|---|---|
| 1 | 00-README.txt | List of files and commands used | 22 files grabbed over SSH by `aigerim` | I'm working from saved logs, not the live server. |
| 2 | 01 (`hostnamectl`), 02 (`uptime`) | OS version and uptime | Ubuntu 24.04.1 VM (VirtualBox), up 6d 23h | Server last booted around Nov 9 09:50 and hasn't rebooted since. |
| 3 | 05 (`systemctl --failed`) | Failed services | Two failed units: `legacy-sync` and `nightly-report` | The ticket only mentions `nightly-report`, but I'll check both. |
| 4 | 06 (`systemctl list-timers`) | If the timer is dead | `nightly-report.timer` is active and running every 2 mins | Timer is fine The problem is with the script it triggers. |
| 5 | 08 (`systemctl cat`) | Typos or bad paths in the service/timer | Points to `/usr/local/bin/nightly-report.sh`, runs every 2 mins | The service and timer configs look right. |
| 6 | 07 (`systemctl status`) | Exit code and error message | `status=203/EXEC`, "No such file or directory", 2ms CPU | systemd couldn't even start the script. Maybe the file is missing? |
| 7 | 13 (`ls -l /usr/local/bin`) | Check if the file is actually there | File exists (`-rwxr-xr-x`, root, 91 bytes, edited Nov 9 14:20) |Not missing, and permissions are fine (would say "Permission denied" otherwise). The Nov 9 14:20 timestamp is a big clue. |
| 8 | 09 (`journalctl`) | When the errors started | Worked fine until Nov 9 14:18, broke at 14:20 | Something changed between 14:18 and 14:20—right when the file was modified. |
| 9 | 14 (`ls /var/log`), 15 (`tail` log) | Latest log entries | Last entry was Nov 9 14:18 (only had `date` and `df`, no `free -h`) | Logging stopped because the script stopped. The new version added `free -h`. |
| 10 | 03 (`df -h`), 04 (`free -h`) | Full disk or out of memory | Disk is 48% used (5.8G free), 1.5Gi RAM free | Resources are fine. (Plus, a full disk wouldn't give a 203/EXEC error). |
| 11 | 10 & 11 (`legacy-sync` logs) | If the other failed service is related | Runs `/bin/false` ("no longer used") once at every boot since Sept | Unrelated noise. It fails at boot, while `nightly-report` worked fine for hours after boot. |
| 12 | 21 (`apt` history) | Recent system updates | Full upgrade + reboot on Nov 9 around 09:40 | Not the cause. The script worked for ~4.5 hours after the reboot. |
| 13 | 12 (`journalctl -p err`) | Any other system errors | Just VM guest noise (`vboxguest`), `legacy-sync`, and `nightly-report` | Nothing else relevant here. |
| 14 | 22 (`bash` history) | What `aigerim` did on Nov 9 | Copied `~/nightly-report.sh` to `/usr/local/bin/`, ran `chmod` twice, tested it | Confirms the file was replaced at 14:20. Running `chmod` didn't help because permissions weren't the issue. |
| 15 | 16 (`cat nightly-report.sh`) | Syntax errors in the script | 3 normal-looking lines (`#!/bin/bash`, `date`, `df -h /`, `free -h`) | The code looks completely normal, so there must be hidden characters. |
| 16 | 19 (Manual run) | What happens when run manually | Fails with `cannot execute: required file not found` | Rules out systemd completely. This error usually means the `#!` interpreter line is broken. |
| 17 | 20 (`ls -l /bin/bash`) | Check if `/bin/bash` is missing | Exists and is executable | `/bin/bash` is there, so the script's first line must be pointing to something slightly different. |
| 18 | 17 (`cat -A`) | Hidden characters | Every line ends with `^M$` (including `#!/bin/bash^M$`) |The file has Windows line endings (CRLF). |
| 19 | 18 (`od -c`) | Confirm exact bytes on line 1 | Shows `# ! / b i n / b a s h \r \n` (91 bytes total vs 88 expected) | Linux tries to find an interpreter literally named `/bin/bash\r`, which doesn't exist. |

## Root Cause
On Nov 9 at 14:20, `/usr/local/bin/nightly-report.sh` was overwritten with a file saved using Windows (CRLF) line endings. Because of the extra `\r` character at the end of the first line, Linux looks for `/bin/bash\r` instead of `/bin/bash`, causing it to fail with "No such file or directory" (`203/EXEC`).

## How to Fix It
Log in as `aigerim` on `office-srv` and run these commands:

```bash
sudo cp -p /usr/local/bin/nightly-report.sh /root/nightly-report.sh.crlf

sed -i 's\(/\r\)\$//' ~/nightly-report.sh

sudo systemctl reset-failed nightly-report.service
sudo systemctl start nightly-report.service
```

## How to Verify
Check file format: Run `file /usr/local/bin/nightly-report.sh` and `cat -A /usr/local/bin/nightly-report.sh`. It should say `ASCII text executable` (no CRLF) and lines should end in `$` without `^M`.
Check file size: Run `stat -c '%s bytes, mode %a, owner %U' /usr/local/bin/nightly-report.sh`. It should now be `88 bytes, mode 755, owner root`.
Test run: Run `sudo /usr/local/bin/nightly-report.sh; echo $?`. It should return `0` with no errors.
Check service & logs: Run `systemctl status nightly-report.service` to verify `status=0/SUCCESS`, and check `tail -n 12 /var/log/nightly-report.log` to confirm the new date, `df`, and `free -h` outputs are logging.
Check timer: Wait 5 minutes and run `journalctl -u nightly-report.service --since "10 min ago"` to make sure it runs cleanly every 2 minutes. `systemctl --failed` should now only show `legacy-sync.service`.

After a Reboot:
Since the fix is saved directly to the file on disk and the timer is enabled, it will survive a reboot. To double-check after running `sudo reboot`:
1. Wait ~3 minutes after logging back in.
2. Run `systemctl list-timers nightly-report.timer` to verify `LAST` and `NEXT` are after your `uptime -s` boot time.
3. Run `tail -n 12 /var/log/nightly-report.log` to confirm fresh logs are still coming in.

## Notes for the Accountant
Missing data: Logs between Nov 9 (14:20) and the fix cannot be recovered since the script only captures live system stats.
Follow-up tickets: 
  * `legacy-sync.service` can be disabled once the owner approves.
    * The timer is still set to run every 2 minutes for testing and will need to be switched to a real nightly schedule (`OnCalendar=`) later.
