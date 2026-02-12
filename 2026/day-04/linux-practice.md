# Day 04 – Linux Fundamentals Lab: Monitoring Processes and Services

## Checking Processes

### 1) Kernel / Host Info

bash
uname -a

Output:
Linux ip-172-31-16-26 6.14.0-1018-aws #18~24.04.1-Ubuntu SMP Mon Nov 24 19:46:27 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux


| Part               | Meaning                                           |
| ------------------ | ------------------------------------------------- |
| Linux              | Kernel name                                       |
| ip-172-31-16-26    | Hostname (machine name)                           |
| 6.14.0-1018-aws    | Kernel version                                    |
| #18~24.04.1-Ubuntu | Kernel build info (build number, SMP, build date) |
| x86_64             | CPU architecture (64-bit Intel/AMD)               |
| GNU/Linux          | Operating system                                  |

---

### 2) Glimpses of Processes with Highest CPU Utilization

**Command explanations:**

* `ps` = Shows information about running processes (not live like `top`)
* `-e` = Shows all processes on the system
* `-o` = Custom output format
* `ps aux` = Displays all running processes with detailed information

#### Commands and Outputs
$ps
$ps -m
$ps --sort=-%cpu
$ps aux
### 3) Find Process IDs Using Process Name
$pgrep -a bash | head
Example:
21694 -bash

> PID for login Bash shell session

### 4) Non-Interactive Snapshot of Process Activity
top
**Purpose:**
Real-time, interactive process and system resource monitor.

Shows:

* CPU usage
* Memory usage
* Running and sleeping processes
* Load averages

## Managing and Checking Services (systemd)

### 5) View Service Unit Files on Disk
systemctl list-unit-files
**Overall Purpose:**
Lists all systemd unit files that exist on disk, regardless of whether they are enabled or running.

Shows what services are installed/available.

Directories searched:

* `/etc/systemd/system/`
* `/lib/systemd/system/`
* `/usr/lib/systemd/system/`

Filter only services:

```bash
systemctl list-unit-files --type=service
### 6) Verify Service Status (Expected Failure)
systemctl status apache2
Output:
Unit apache2.service could not be found.

**Conclusion:**
Apache is not installed on this system.

## Log File Review

### 7) Journal Logs

General system logs:


journalctl
SSH service logs:
journalctl -u ssh

Last 10 log entries:


journalctl -n 10

### 8) View Traditional Log File Activity

tail -n 20 /var/log/dpkg.log

**Purpose:**
View recent package management activity and system changes.

## Key Learning Outcomes

* Identified system and kernel details
* Monitored CPU and memory usage
* Found and analyzed running processes
* Used systemd to inspect installed services
* Verified service availability and status
* Read and filtered system logs
* Practiced thinking like a Linux system administrator

---

 Day 04 focused on building real-world Linux administration skills through hands-on process and service monitoring.
