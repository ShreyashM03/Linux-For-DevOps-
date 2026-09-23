
# Process Management

## 📚 Introduction

**Process Management** refers to monitoring, controlling, prioritizing, and terminating processes running on a Linux system.

A **process** is a running instance of a program that uses system resources such as CPU, memory, and disk I/O.

### 🔹 Why Process Management is Important in DevOps

- Monitor application performance
- Troubleshoot high CPU and memory usage
- Manage background services
- Identify and terminate unwanted processes
- Monitor server health
- Troubleshoot application failures
- Optimize resource utilization
- Manage production workloads

---

## 📑 Table of Contents

1. [Introduction](#-introduction)
2. [Types of Processes](#-1-types-of-processes)
3. [Process Identifiers (PID)](#-2-process-identifiers-pid)
4. [Working with Jobs](#-3-working-with-jobs)
5. [Background and Foreground Jobs](#-4-background-and-foreground-jobs)
6. [Monitoring Processes](#-5-monitoring-processes)
7. [Process Information](#-6-process-information)
8. [Process States](#-7-process-states)
9. [Killing Processes](#-8-killing-processes)
10. [Niceness and Process Priority](#-9-niceness-and-process-priority)
11. [Systemd and Service Management](#-10-systemd-and-service-management)
12. [CPU and Memory Monitoring](#-11-cpu-and-memory-monitoring)
13. [Process Tree](#-12-process-tree)
14. [Find and Filter Processes](#-13-find-and-filter-processes)
15. [Real-World DevOps Use Cases](#-14-real-world-devops-use-cases)
16. [Process Troubleshooting](#-15-process-troubleshooting)
17. [Hands-On Practice](#-16-hands-on-practice)
18. [Important Interview Questions](#-17-important-interview-questions)
19. [Quick Revision](#-18-quick-revision)
20. [Chapter Checklist](#-chapter-checklist)

---

## 🔹 1. Types of Processes

### 1. Foreground Process

A process that runs in the foreground and occupies the terminal until it completes or is suspended.

```bash
sleep 30
```

### 2. Background Process

A process that runs in the background, allowing the terminal to accept other commands.

```bash
sleep 30 &
```

### 3. Daemon Process

A background process that provides services, often managed by the system or service manager.

Examples:

- `sshd` – SSH service
- `cron` – Scheduled task service
- `nginx` – Web server
- `systemd` – System and service manager

> **Important:** Not every daemon runs with root privileges. Processes run with the permissions of their configured user.

### 4. Orphan Process

A process whose original parent has terminated. It is adopted by another process, commonly PID 1 or a subreaper.

### 5. Zombie Process

A process that has finished execution but still has an entry in the process table because its parent has not collected its exit status.

---

## 🆔 2. Process Identifiers (PID)

Every process in Linux has a unique Process ID (PID) while it exists.

| Term | Meaning |
|------|---------|
| PID | Process ID |
| PPID | Parent Process ID |
| UID | User ID running the process |
| GID | Group ID of the process |
| PID 1 | Initial system process or service manager |

### 🔍 Display current shell PID

```bash
echo $$
```

### Display parent shell PID

```bash
echo $PPID
```

### Display PID of the last background process

```bash
sleep 30 &
echo $!
```

### Display the current user's ID

```bash
id
```

### Find the PID of a running process

```bash
pgrep nginx
```

> **Note:** PID 1 is commonly `systemd` on modern Linux systems, although other init systems may be used.

---

## 🛠️ 3. Working with Jobs

Jobs are shell-managed tasks associated with the current terminal session.

### 🔹 Listing Jobs

```bash
jobs
```

### Common Options

| Option | Description |
|--------|-------------|
| `-r` | List running jobs |
| `-s` | List stopped jobs |
| `-p` | Display process IDs |
| `-l` | Display detailed information |

### Example: Start a process

```bash
sleep 30
```

### Stop a foreground process

```text
Ctrl + Z
```

Suspends the current foreground job.

### Cancel a foreground process

```text
Ctrl + C
```

Sends an interrupt signal to the foreground process.

### Terminate a job

```bash
kill %1
```

Terminates job number 1.

> **Difference:** A job ID (such as `%1`) belongs to the shell, while a PID identifies the operating-system process.

---

## 🔄 4. Background and Foreground Jobs

### Run a command in the background

```bash
sleep 30 &
```

### Move a stopped job to the background

```bash
bg %1
```

### Bring a job to the foreground

```bash
fg %1
```

### List jobs with PIDs

```bash
jobs -l
```

### Example Workflow

```bash
sleep 100
```

Press:

```text
Ctrl + Z
```

Then:

```bash
bg
```

Bring it back:

```bash
fg
```

### Run a command without occupying the terminal

```bash
sleep 60 &
```

> **DevOps Use Case:** Background jobs are useful for testing scripts, running long-running commands, and executing tasks while continuing other terminal work.

---

## 🔍 5. Monitoring Processes

### 1. List processes for the current shell

```bash
ps
```

### 2. Display all processes

```bash
ps aux
```

### 3. Display detailed process information

```bash
ps -ef
```

### 4. Display processes for a specific user

```bash
ps -u username
```

### 5. Real-time process monitoring

```bash
top
```

### 6. Interactive process monitoring

```bash
htop
```

> `htop` may need to be installed separately.

### 7. Display system uptime and load

```bash
uptime
```

### 8. Display logged-in users and load

```bash
w
```

### 9. Monitor process resource usage

```bash
top
```

### Important `top` Shortcuts

| Key | Action |
|-----|--------|
| `P` | Sort by CPU usage |
| `M` | Sort by memory usage |
| `k` | Send a signal to a process |
| `q` | Quit |
| `1` | Show individual CPU information |

---

## 📊 6. Process Information

### Display detailed information about a process

```bash
ps -p 4407 -f
```

### Display process details using `/proc`

```bash
cat /proc/4407/status
```

### View the executable path

```bash
readlink -f /proc/4407/exe
```

### View the command used to start a process

```bash
tr '\0' ' ' < /proc/4407/cmdline
echo
```

### View the open files of a process

```bash
sudo lsof -p 4407
```

> **DevOps Use Case:** `/proc` provides information about running processes, resource usage, and process configuration.

---

## 🔹 7. Process States

| State | Meaning |
|-------|---------|
| `R` | Running or runnable |
| `S` | Interruptible sleep |
| `D` | Uninterruptible sleep |
| `T` | Stopped or traced |
| `Z` | Zombie |
| `I` | Idle kernel thread (where supported) |

### Display process states

```bash
ps aux
```

### Display process state and command

```bash
ps -eo pid,ppid,stat,cmd
```

### Find zombie processes

```bash
ps -eo pid,ppid,stat,cmd | awk '$3 ~ /Z/'
```

### Important Note: Zombie Processes

A zombie process has already finished execution. Sending `SIGKILL` to the zombie itself does not remove it.

Possible actions:

1. Identify the parent process (PPID).
2. Investigate why the parent has not collected the child's exit status.
3. Fix or restart the responsible parent process when appropriate.

---

## 🛑 8. Killing Processes

Linux uses signals to control processes.

### Common Signals

| Signal | Number | Purpose |
|--------|--------|---------|
| `SIGTERM` | 15 | Request graceful termination |
| `SIGKILL` | 9 | Force termination |
| `SIGINT` | 2 | Interrupt process |
| `SIGHUP` | 1 | Hangup; behavior depends on application |
| `SIGSTOP` | 19 (commonly) | Stop process; cannot be caught |
| `SIGCONT` | 18 (commonly) | Continue stopped process |

### Gracefully terminate a process

```bash
kill 4407
```

Equivalent to sending `SIGTERM` by default.

### Explicitly send SIGTERM

```bash
kill -15 4407
```

### Forcefully terminate a process

```bash
kill -9 4407
```

> **Best Practice:** Try `SIGTERM` before `SIGKILL`. Forced termination can prevent cleanup and may cause data loss.

### Send a signal by process name

```bash
pkill -TERM nginx
```

### Kill a process by PID

```bash
kill 4407
```

### Check available signals

```bash
kill -l
```

### Check whether a process exists

```bash
kill -0 4407
```

`kill -0` checks whether a signal could be sent; it does not terminate the process.

---

## ⚡ 9. Niceness and Process Priority

**Niceness** influences the scheduling priority of normal processes.

- Range: `-20` (highest scheduling priority) to `+19` (lowest scheduling priority).
- Default: Usually `0`.
- A lower nice value generally gives a process more favorable CPU scheduling priority.

### 1. Start a process with a nice value

```bash
nice -n 10 sleep 30
```

### 2. View process niceness

```bash
ps -eo pid,ni,comm
```

### 3. Change process niceness

```bash
renice -n 5 -p 4409
```

> **Note:** The exact new nice value is set to `5` in this example. Increasing priority (decreasing the nice value) generally requires elevated privileges.

### 4. Run a command with higher priority

```bash
sudo nice -n -5 command
```

> Use elevated privileges carefully. Niceness affects CPU scheduling, not the process's permissions or resource limits.

---

## ⚙️ 10. Systemd and Service Management

In many Linux distributions, `systemd` manages system services and their lifecycle.

### Check service status

```bash
sudo systemctl status nginx
```

### Start a service

```bash
sudo systemctl start nginx
```

### Stop a service

```bash
sudo systemctl stop nginx
```

### Restart a service

```bash
sudo systemctl restart nginx
```

### Reload service configuration

```bash
sudo systemctl reload nginx
```

### Enable service at boot

```bash
sudo systemctl enable nginx
```

### Disable service at boot

```bash
sudo systemctl disable nginx
```

### List running services

```bash
systemctl list-units --type=service --state=running
```

### View service logs

```bash
sudo journalctl -u nginx
```

### Follow service logs

```bash
sudo journalctl -u nginx -f
```

### Check service processes

```bash
systemctl status nginx
```

> **DevOps Use Case:** Manage web servers, application services, and background workers in Linux environments.

---

## 📈 11. CPU and Memory Monitoring

### Check memory usage

```bash
free -h
```

### Check CPU and memory using top

```bash
top
```

### Check load average

```bash
uptime
```

### Display CPU information

```bash
lscpu
```

### Display memory information

```bash
cat /proc/meminfo
```

### Find processes using the most CPU

```bash
ps -eo pid,comm,%cpu --sort=-%cpu | head
```

### Find processes using the most memory

```bash
ps -eo pid,comm,%mem --sort=-%mem | head
```

### Check disk I/O statistics

```bash
iostat
```

> `iostat` is provided by the `sysstat` package on many distributions.

---

## 🌳 12. Process Tree

A process tree shows the parent-child relationship between processes.

### Display process tree

```bash
pstree
```

### Display PIDs in the process tree

```bash
pstree -p
```

### Display the parent-child relationship

```bash
ps -ef --forest
```

### Find the parent process

```bash
ps -o pid,ppid,cmd -p 4407
```

> **DevOps Use Case:** Identify which parent process launched an application, worker, or child process during troubleshooting.

---

## 🔎 13. Find and Filter Processes

### Find a process by name

```bash
pgrep nginx
```

### Find processes by name with details

```bash
pgrep -a nginx
```

### Find process using a specific port

```bash
sudo lsof -i :80
```

### Alternative using ss

```bash
sudo ss -ltnp 'sport = :80'
```

### Find process by command name

```bash
pidof nginx
```

### Search running processes

```bash
ps aux | grep nginx
```

> **Note:** `pgrep` is usually more reliable than a basic `grep` search because it avoids matching the search command itself.

---

## 🚀 14. Real-World DevOps Use Cases

### 🔹 Use Case 1: Find High CPU Usage

**Problem:** An application is consuming too much CPU.

```bash
top
```

Find the process with high CPU usage:

```bash
ps -eo pid,comm,%cpu --sort=-%cpu | head
```

**Action:**

1. Identify the process.
2. Check application logs.
3. Investigate resource usage.
4. Restart or adjust the application only when appropriate.

---

### 🔹 Use Case 2: Find High Memory Usage

```bash
free -h
```

```bash
ps -eo pid,comm,%mem --sort=-%mem | head
```

**Action:**

- Identify memory-intensive processes.
- Check whether memory usage is expected.
- Review application configuration and logs.
- Investigate potential memory leaks.

---

### 🔹 Use Case 3: Check Whether Nginx is Running

```bash
sudo systemctl status nginx
```

Check its process:

```bash
pgrep -a nginx
```

Check port 80:

```bash
sudo ss -ltnp 'sport = :80'
```

---

### 🔹 Use Case 4: Gracefully Stop a Stuck Application

Find the process:

```bash
pgrep -a application-name
```

Send SIGTERM:

```bash
kill -15 PID
```

Check whether it is still running:

```bash
ps -p PID
```

If necessary, investigate before using:

```bash
kill -9 PID
```

> **Best Practice:** Replace `PID` with the actual process ID. Avoid blindly terminating production processes.

---

### 🔹 Use Case 5: Monitor a Running Process

```bash
watch -n 2 'ps -p 4407 -o pid,ppid,stat,%cpu,%mem,cmd'
```

> `watch` reruns the command every 2 seconds. Install it if it is unavailable.

---

### 🔹 Use Case 6: Check for Port Conflicts

**Problem:** An application cannot start because a port is already in use.

```bash
sudo ss -ltnp 'sport = :8080'
```

Identify the process, then decide whether it should be stopped or whether the application configuration should change.

---

## 🛠️ 15. Process Troubleshooting

### Problem 1: Application is not responding

**Steps:**

```bash
ps aux | grep application-name
```

```bash
top
```

```bash
sudo journalctl -u application-service -n 50
```

Check CPU, memory, process state, and application logs.

---

### Problem 2: High CPU Usage

```bash
top
```

```bash
ps -eo pid,comm,%cpu --sort=-%cpu | head
```

Investigate the process before terminating it.

---

### Problem 3: High Memory Usage

```bash
free -h
```

```bash
ps -eo pid,comm,%mem --sort=-%mem | head
```

Review memory usage, application logs, and system behavior.

---

### Problem 4: Permission Denied

Check the process owner:

```bash
ps -o user,pid,cmd -p 4407
```

Check file permissions:

```bash
ls -l /path/to/file
```

> A process may require specific user permissions to access files or perform administrative operations.

---

### Problem 5: Process Keeps Restarting

Check service status:

```bash
sudo systemctl status application.service
```

Check recent logs:

```bash
sudo journalctl -u application.service -n 100
```

Check restart configuration:

```bash
systemctl cat application.service
```

Investigate the root cause rather than repeatedly killing the process.

---

## 🧪 16. Hands-On Practice

### Task 1: Start a background process

```bash
sleep 300 &
```

Check jobs:

```bash
jobs -l
```

---

### Task 2: Find the process PID

```bash
pgrep -a sleep
```

---

### Task 3: Display process details

```bash
ps -p PID -f
```

Replace `PID` with the actual process ID.

---

### Task 4: Change process priority

```bash
renice -n 10 -p PID
```

---

### Task 5: Stop the process

```bash
kill PID
```

---

### Task 6: Check system resources

```bash
free -h
```

```bash
uptime
```

```bash
top
```

---

### Task 7: Explore the process tree

```bash
pstree -p
```

---

### Task 8: Monitor a process

```bash
watch -n 2 'ps -p PID -o pid,ppid,stat,%cpu,%mem,cmd'
```

---

## 🧠 17. Important Interview Questions

### Basic Questions

1. What is process management in Linux?
2. What is the difference between a process and a job?
3. What is a PID and PPID?
4. What is the difference between foreground and background processes?
5. What is the use of `ps`?
6. What is the difference between `ps aux` and `ps -ef`?
7. What is the purpose of `top` and `htop`?
8. What are daemon processes?
9. What is a zombie process?
10. What is an orphan process?
11. What are common Linux process states?
12. What is the use of `jobs`, `bg`, and `fg`?

### Signal and Priority Questions

13. What is the difference between SIGTERM and SIGKILL?
14. Why should SIGTERM be preferred before SIGKILL?
15. What is the use of `kill -0`?
16. What is niceness in Linux?
17. What is the range of niceness values?
18. How do you change the priority of a running process?
19. What happens when you press Ctrl+C?
20. What happens when you press Ctrl+Z?

### DevOps Scenario-Based Questions

21. How do you troubleshoot high CPU usage on a Linux server?
22. How do you find the process consuming the most memory?
23. An application is not responding. How do you troubleshoot it?
24. How do you find which process is using port 8080?
25. How do you check whether Nginx is running?
26. How do you stop a process gracefully?
27. What would you do if a process keeps restarting?
28. How do you check application service logs using systemd?
29. How do you identify a zombie process?
30. How do you monitor a running process in real time?
31. How do you identify parent and child processes?
32. Why might a process work manually but fail when started as a service?
33. How do you investigate a service that consumes excessive memory?
34. How do you safely terminate a process in production?
35. What is the difference between cron jobs and systemd services?

---

## 📝 18. Quick Revision

| Command | Purpose |
|---------|---------|
| `ps aux` | List running processes |
| `ps -ef` | Display detailed processes |
| `top` | Real-time process monitoring |
| `htop` | Interactive process monitoring |
| `jobs` | List shell jobs |
| `bg` | Run a stopped job in the background |
| `fg` | Bring a job to the foreground |
| `kill PID` | Send SIGTERM by default |
| `kill -9 PID` | Send SIGKILL |
| `pgrep nginx` | Find process IDs by name |
| `pkill nginx` | Send a signal to matching processes |
| `nice` | Start with a specified niceness |
| `renice` | Change process niceness |
| `pstree` | Display process hierarchy |
| `free -h` | Display memory usage |
| `uptime` | Display uptime and load average |
| `systemctl status` | Check service status |
| `journalctl -u` | View service logs |
| `lsof -i :8080` | Find processes using a port |

---

## 🎯 Key DevOps Concepts to Remember

- **PID:** Identifies a running process.
- **PPID:** Identifies the parent process.
- **SIGTERM:** Requests graceful termination.
- **SIGKILL:** Forces termination and cannot be caught or ignored.
- **Zombie:** Completed process awaiting parent cleanup.
- **Niceness:** Influences CPU scheduling priority.
- **Systemd:** Manages services and their lifecycle.
- **top:** Monitors system processes in real time.
- **ps:** Displays process information.
- **journalctl:** Reads systemd journal logs.

---

## ✅ Chapter Checklist

- [ ] Understand Linux processes
- [ ] Learn PID and PPID
- [ ] Practice foreground and background jobs
- [ ] Use `ps`, `top`, and `htop`
- [ ] Understand process states
- [ ] Learn Linux signals
- [ ] Practice graceful process termination
- [ ] Understand niceness
- [ ] Practice systemd service management
- [ ] Monitor CPU and memory
- [ ] Find processes using ports
- [ ] Troubleshoot application processes
- [ ] Practice scenario-based interview questions

---

## 🚀 Practical DevOps Project Idea

### Linux Process Monitoring Script

Create a Bash script that checks CPU and memory usage.

```bash
#!/bin/bash

echo "===== Process Monitoring ====="
echo "Hostname: $(hostname)"
echo "Date: $(date)"

echo ""
echo "===== Memory Usage ====="
free -h

echo ""
echo "===== Top CPU Processes ====="
ps -eo pid,comm,%cpu --sort=-%cpu | head -6

echo ""
echo "===== Top Memory Processes ====="
ps -eo pid,comm,%mem --sort=-%mem | head -6
```

Save as:

```bash
nano process-monitor.sh
```

Make executable:

```bash
chmod +x process-monitor.sh
```

Run:

```bash
./process-monitor.sh
```

### 💡 What You Learn

- Bash scripting
- Process monitoring
- CPU and memory analysis
- Linux commands
- Automation fundamentals

**Next Chapter:** Linux Networking Commands 🌐
