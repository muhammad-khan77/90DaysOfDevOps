# Linux Troubleshooting Runbook

## Day 5 Goal

Run a focused troubleshooting drill by selecting an active service and performing a rapid health assessment. Capture CPU, memory, disk, and network metrics, review logs, and document findings. Convert the exercise into a repeatable incident-response routine.

---

## What Is a Runbook?

A runbook is a concise, step-by-step operational checklist used during incidents. It documents:

* Commands executed
* Observations
* Expected results
* Escalation or next steps

It is designed to be followed quickly under pressure.

---

# Service Under Investigation

**Service:** SSH
**Process:** sshd
**Reason:** Core service, always running, easy to validate via logs and network behavior.

---

# 1) System Environment Details

## Kernel Information

```bash
uname -a
```

**Observation:** Kernel version and architecture confirmed. No abnormal kernel changes detected.

---

## OS Release

```bash
cat /etc/os-release
```

**Observation:** Ubuntu 24.04.3 LTS confirmed. Package locations and log paths verified.

---

# 2) Filesystem Sanity

Filesystem sanity ensures disk structures match the OS view of stored data.

### Why It Matters

* Prevents data loss
* Preserves system stability
* Detects corruption early

### When Needed

* After crashes
* After improper shutdowns
* When disk errors occur

---

# 3) Filesystem Write Test

```bash
mkdir /tmp/runbook-demo
cp /etc/hosts /tmp/runbook-demo/hosts-copy
ls -l /tmp/runbook-demo
```

**Observation:** Filesystem writable. Permissions and ownership normal.

---

# 4) Disk Utilization Overview

```bash
df -h
```

**Observation:**

* Root filesystem at 13% usage
* No disk pressure
* Healthy capacity levels

---

# 5) CPU & Memory Snapshot

```bash
top -b -n 1 | head -20
```

**Observation:**

* Load average: 0.00
* CPU idle: 100%
* No abnormal process activity
* Memory stable

System healthy and under no stress.

---

# 6) SSH Resource Usage

```bash
ps -o pid,pcpu,pmem,comm -C sshd
```

**Observation:**

* CPU usage <1%
* Memory usage minimal
* No runaway processes

---

# 7) Memory Capacity Overview

```bash
free -h
```

**Observation:**

* Adequate free and available memory
* No swap pressure

---

# 8) Log Storage Review

```bash
sudo du -sh /var/log
```

**Observation:** Logs consuming reasonable space. No excessive growth.

---

# 9) I/O Activity Check

```bash
vmstat 1 5
```

**Observation:**

* Low run queue
* No I/O wait spikes
* System not disk-bound

---

# 10) Listening Ports

```bash
sudo ss -tulpn | grep ssh
```

**Observation:** sshd listening on port 22 (IPv4 & IPv6). Service operating normally.

---

# 11) Network Connectivity Test

```bash
curl -I localhost
```

**Observation (Failure Case):**

* Connection refused on port 80
* Indicates no web server running

**Observation (Success Case):**

* HTTP 200 OK
* Local network stack responsive

---

# 12) SSH Service Logs

```bash
journalctl -u ssh -n 20
```

**Observation:**

* Normal login and disconnect activity
* No service crashes
* No authentication anomalies

---

# 13) Authentication Logs

```bash
tail -n 20 /var/log/auth.log
```

**Observation:**

* Routine CRON activity
* No burst of failed logins
* No suspicious behavior detected

---

# CPU Spike Simulation Exercise

## Scenario

High system load and degraded performance.

Goal: Detect, validate, and remediate CPU spike.

---

## Step 1: Generate Controlled CPU Load

### Single-Core Spike

```bash
yes > /dev/null &
```

Stop:

```bash
killall yes
```

---

### Multi-Core Spike

```bash
for i in {1..4}; do yes > /dev/null & done
```

---

## Step 2: Detect the Spike

```bash
uptime
```

Observe load average increase.

```bash
top
```

Observe high CPU usage and offending process.

---

## Step 3: Identify Offending Process

```bash
ps -eo pid,ppid,pcpu,pmem,comm --sort=-pcpu | head
```

Confirm high %CPU process.

```bash
mpstat -P ALL 1 3
```

Confirm per-core utilization.

---

## Step 4: Assess Blast Radius

### Memory

```bash
free -h
```

Memory stable.

### I/O

```bash
vmstat 1 5
```

High run queue, low I/O wait → CPU-bound issue.

---

## Step 5: Log Review

```bash
journalctl -n 20
```

Check for OOM killer or kernel panic messages.

---

## Step 6: Mitigation

```bash
pkill yes
```

or

```bash
kill -9 <pid>
```

---

## Step 7: Confirm Recovery

```bash
top
uptime
```

**Expected Result:**

* Load average decreases
* CPU idle restored
* System stabilized

---

# Final Incident Summary

* Baseline system healthy
* CPU spike successfully simulated
* Offending process identified
* System confirmed CPU-bound
* No memory or disk impact
* Process terminated
* System returned to stable state

---

# Outcome

This runbook establishes a repeatable troubleshooting workflow:

1. Capture baseline
2. Detect anomaly
3. Identify root cause
4. Assess impact
5. Mitigate
6. Confirm recovery

Designed for real-world production incident response.
