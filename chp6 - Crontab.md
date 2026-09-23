
#  Chapter 6: Cron Jobs & Crontab

## 📚 Introduction
**Cron:** A time-based job scheduler in Linux used to automate repetitive tasks at specific intervals.

**Crontab:** A configuration file that contains scheduled cron jobs and their execution times.

## 📑 Table of Contents

- [ Introduction](#-introduction)
- [ Cron Job Syntax](#-1-cron-job-syntax)
- [ Crontab Commands](#-2-crontab-commands)
- [ Cron Job Operators](#-3-cron-job-operators)
- [ Common Cron Job Examples](#-4-common-cron-job-examples)
- [ Run a Shell Script Using Cron](#-5-run-a-shell-script-using-cron)
- [ Cron Environment Variables](#-6-cron-environment-variables)
- [ Redirect Cron Output to Log Files](#-7-redirect-cron-output-to-log-files)
- [ Automated Log Cleanup](#-8-automated-log-cleanup)
- [ Automated Backup Using Cron](#-9-automated-backup-using-cron)
- [ Check Cron Service Status](#-10-check-cron-service-status)
- [ View Cron Logs](#-11-view-cron-logs)
- [ Cron Job Troubleshooting](#-12-cron-job-troubleshooting)
- [ Cron Security Best Practices](#-13-cron-security-best-practices)
- [ System-Wide Cron Jobs](#-14-system-wide-cron-jobs)
- [ Cron vs Anacron](#-15-cron-vs-anacron)
- [ Cron vs Systemd Timers](#-16-cron-vs-systemd-timers)
- [ Hands-On Practice](#-17-hands-on-practice)
- [ Important Interview Questions](#-18-important-interview-questions)
- [ Quick Revision](#-19-quick-revision)
- [ Chapter 6 Checklist](#-chapter-6-checklist)

  ---

### 🔹 Common DevOps Use Cases

- Automated application backups
- Log cleanup and rotation
- Server monitoring
- Database backups
- Temporary file cleanup
- Scheduled scripts
- Health checks
- Automated reports

---

## ⏰ 1. Cron Job Syntax

```bash
* * * * * command_to_run
- - - - -
| | | | |
| | | | +---- Day of the week (0 - 7)
| | | +------ Month (1 - 12)
| | +-------- Day of the month (1 - 31)
| +---------- Hour (0 - 23)
+------------ Minute (0 - 59)
```

### 🔹 Day of the Week

| Value | Day |
|-------|-----|
| `0` or `7` | Sunday |
| `1` | Monday |
| `2` | Tuesday |
| `3` | Wednesday |
| `4` | Thursday |
| `5` | Friday |
| `6` | Saturday |

> **Note:** Cron uses the server's configured timezone unless otherwise configured.

---

## 🛠️ 2. Crontab Commands

### Edit cron jobs

```bash
crontab -e
```

Opens the current user's crontab for editing.

### View current cron jobs

```bash
crontab -l
```

Lists scheduled jobs for the current user.

### Remove all cron jobs

```bash
crontab -r
```

⚠️ **Warning:** This removes all cron jobs belonging to the current user. Confirm before running.

### Edit another user's crontab (root)

```bash
sudo crontab -u username -e
```

### List another user's cron jobs (root)

```bash
sudo crontab -u username -l
```

---

## 📝 3. Cron Job Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `*` | Every value | `* * * * *` |
| `,` | Multiple values | `1,3,5` |
| `-` | Range | `1-5` |
| `/` | Step interval | `*/10` |

### 🔹 Examples

**Every minute**

```bash
* * * * * command
```

**At minute 0 of every hour**

```bash
0 * * * * command
```

**Every 10 minutes**

```bash
*/10 * * * * command
```

**Monday to Friday**

```bash
* * * * 1-5 command
```

**At 8 AM and 8 PM**

```bash
0 8,20 * * * command
```

---

## 🚀 4. Common Cron Job Examples

| Expression | Meaning |
|-------------|---------|
| `0 0 * * * ./backup.sh` | Run backup daily at midnight |
| `0 6,18 * * * ./backup.sh` | Run backup at 6 AM and 6 PM |
| `0 */6 * * * ./monitor.sh` | Run monitoring every 6 hours |
| `*/10 * * * * ./script.sh` | Run script every 10 minutes |
| `30 8 * * * ./script.sh` | Run script daily at 8:30 AM |
| `0 0 * * 0 ./backup.sh` | Run backup every Sunday at midnight |
| `0 9 * * 1-5 ./report.sh` | Run report on weekdays at 9 AM |

### ⚠️ Important Correction

Your original example:

```bash
30 8 19 jun * touch example.txt
```

Runs at **8:30 AM on June 19**, every year (when the day-of-week field matches cron's OR semantics for a restricted day-of-month and day-of-week; here day-of-week is `*`, so it runs on June 19).

To run a script daily at 8:30 AM:

```bash
30 8 * * * touch example.txt
```

---

## 📂 5. Run a Shell Script Using Cron

### Step 1: Create a script

```bash
nano backup.sh
```

### Step 2: Add script content

```bash
#!/bin/bash

echo "Backup started: $(date)"

mkdir -p "$HOME/cron-backups"

tar -czf "$HOME/cron-backups/project-$(date +%F-%H%M).tar.gz" "$HOME/project"

echo "Backup completed: $(date)"
```

> Update `$HOME/project` to the actual project directory you want to back up.

### Step 3: Give execute permission

```bash
chmod +x backup.sh
```

### Step 4: Test the script manually

```bash
./backup.sh
```

### Step 5: Add the script to crontab

```bash
crontab -e
```

Add:

```bash
0 2 * * * /home/username/backup.sh
```

This runs the backup every day at **2:00 AM**.

> **Best Practice:** Use an absolute path to the script rather than relying on the current working directory.

---

## 🔐 6. Cron Environment Variables

Cron has a limited environment compared to an interactive terminal.

### Set PATH explicitly

```bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

### Set the shell

```bash
SHELL=/bin/bash
```

### Set the mail recipient

```bash
MAILTO=""
```

### Example

```bash
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
MAILTO=""

0 2 * * * /home/username/backup.sh
```

> **DevOps Interview Tip:** A script that works manually may fail in cron because of missing PATH variables, incorrect working directories, permissions, or environment variables.

---

## 📋 7. Redirect Cron Output to Log Files

### Save output to a log file

```bash
0 2 * * * /home/username/backup.sh >> /home/username/backup.log 2>&1
```

### Explanation

| Symbol | Meaning |
|--------|---------|
| `>` | Overwrite output file |
| `>>` | Append output to file |
| `2>` | Redirect standard error |
| `2>&1` | Redirect standard error to standard output |

### Separate output and error logs

```bash
0 2 * * * /home/username/backup.sh >> /home/username/backup.log 2>> /home/username/backup-error.log
```

---

## 🧹 8. Automated Log Cleanup

### Create a cleanup script

```bash
nano cleanup.sh
```

```bash
#!/bin/bash

find /home/username/logs \
  -type f \
  -name "*.log" \
  -mtime +7 \
  -delete
```

### Make it executable

```bash
chmod +x cleanup.sh
```

### Schedule daily cleanup at 3 AM

```bash
0 3 * * * /home/username/cleanup.sh
```

> **DevOps Use Case:** Remove log files older than 7 days to reduce disk usage. Test `find` commands without `-delete` before enabling deletion in production.

---

## 💾 9. Automated Backup Using Cron

### Create a backup script

```bash
#!/bin/bash

SOURCE="/home/username/project"
DEST="/home/username/backups"

mkdir -p "$DEST"

tar -czf "$DEST/backup-$(date +%F).tar.gz" "$SOURCE"

echo "Backup completed successfully"
```

### Schedule the backup

```bash
0 1 * * * /home/username/backup.sh >> /home/username/backup.log 2>&1
```

**Execution:** Every day at 1:00 AM.

### Recommended Backup Practices

- Use absolute paths.
- Verify that backups are created successfully.
- Store backups separately from the source directory.
- Protect sensitive data.
- Monitor available disk space.
- Test restoring backups regularly.

---

## 🔎 10. Check Cron Service Status

### Check cron service (Ubuntu/Debian)

```bash
sudo systemctl status cron
```

### Start cron service

```bash
sudo systemctl start cron
```

### Enable cron at boot

```bash
sudo systemctl enable cron
```

### Restart cron service

```bash
sudo systemctl restart cron
```

> **Note:** Service names and commands may differ across Linux distributions. RHEL-based systems commonly use `crond`.

---

## 📜 11. View Cron Logs

### Ubuntu/Debian systems

```bash
grep CRON /var/log/syslog
```

### Follow cron logs

```bash
sudo tail -f /var/log/syslog
```

### RHEL-based systems using systemd

```bash
sudo journalctl -u crond
```

### Follow cron service logs

```bash
sudo journalctl -u cron -f
```

> Log locations depend on the Linux distribution and logging configuration.

---

## 🛠️ 12. Cron Job Troubleshooting

### Problem 1: Script works manually but not in cron

**Possible reasons:**

- Incorrect PATH
- Relative paths
- Missing environment variables
- Permission issues
- Incorrect working directory

**Solution:**

Use absolute paths and define required environment variables.

---

### Problem 2: Cron job is not running

### Check the cron service

```bash
sudo systemctl status cron
```

### Check scheduled jobs

```bash
crontab -l
```

### Check script permissions

```bash
ls -l /home/username/backup.sh
```

### Check script syntax

```bash
bash -n /home/username/backup.sh
```

### Check cron logs

```bash
grep CRON /var/log/syslog
```

---

### Problem 3: Permission denied

### Check file permissions

```bash
ls -l backup.sh
```

### Make the script executable

```bash
chmod +x backup.sh
```

> Cron runs as the user who owns the crontab. Ensure the user has the required permissions.

---

## 🔐 13. Cron Security Best Practices

- Use the least-privileged user required.
- Avoid running scripts as root unnecessarily.
- Secure script and backup permissions.
- Avoid hardcoding passwords in scripts.
- Use absolute paths for commands and files.
- Validate inputs in scripts.
- Review cron jobs regularly.
- Protect sensitive log files.

### Check cron permissions

```bash
ls -l /etc/crontab
```

### View system-wide cron directories

```bash
ls -la /etc/cron.daily/
ls -la /etc/cron.hourly/
ls -la /etc/cron.weekly/
ls -la /etc/cron.monthly/
```

---

## 🧰 14. System-Wide Cron Jobs

### System crontab

```bash
cat /etc/crontab
```

System crontab includes an additional **user field**:

```bash
* * * * * username command
```

### Example

```bash
0 2 * * * root /usr/local/bin/backup.sh
```

> **Difference:** User crontabs generally contain five time fields followed by the command. `/etc/crontab` and files in `/etc/cron.d/` include a username field.

---

## 📅 15. Cron vs Anacron

| Feature | Cron | Anacron |
|---------|------|---------|
| Scheduling | Exact time-based scheduling | Daily or periodic jobs |
| Requires system running | Yes, at scheduled time | Can catch up after downtime |
| Best use | Frequent scheduled tasks | Periodic maintenance on machines not always running |
| Example | Every 10 minutes | Daily cleanup |

> **Interview Tip:** Anacron is useful for periodic jobs that should run even if the machine was powered off when the scheduled time passed.

---

## ⏱️ 16. Cron vs Systemd Timers

| Feature | Cron | Systemd Timer |
|---------|------|---------------|
| Configuration | Crontab | Unit files |
| Logging | Depends on system logging | Integrated with journald |
| Dependencies | Limited | Supports service dependencies |
| Scheduling | Time-based | Time-based and event-related options |
| Use case | Simple recurring jobs | Complex service scheduling |

> **DevOps Tip:** Learn both cron and systemd timers. Many Linux environments use systemd for managing services and scheduled tasks.

---

## 🧪 17. Hands-On Practice

### Task 1: Run a command every minute

```bash
* * * * * date >> /tmp/cron-test.log 2>&1
```

### Task 2: Run a script every 5 minutes

```bash
*/5 * * * * /home/username/test.sh
```

### Task 3: Run a backup daily at midnight

```bash
0 0 * * * /home/username/backup.sh
```

### Task 4: Run a cleanup script every Sunday at 4 AM

```bash
0 4 * * 0 /home/username/cleanup.sh
```

### Task 5: Check execution logs

```bash
tail -f /tmp/cron-test.log
```

---

## 🧠 18. Important Interview Questions

### Basic Questions

1. What is cron in Linux?
2. What is the difference between cron and crontab?
3. Explain the five fields in cron syntax.
4. How do you create a cron job?
5. How do you list existing cron jobs?
6. How do you remove cron jobs?
7. What is the difference between `*`, `,`, `-`, and `/` in cron?
8. How do you schedule a job every 5 minutes?
9. How do you schedule a job at midnight every day?
10. What is the difference between user crontab and `/etc/crontab`?

### DevOps Scenario-Based Questions

11. How would you schedule an automated application backup?
12. Your script works manually but fails in cron. How would you troubleshoot it?
13. How do you redirect cron output and errors to a log file?
14. How would you clean up logs older than 7 days?
15. How do you check whether the cron service is running?
16. How do you schedule a job to run as a specific user?
17. How do you troubleshoot a cron job that is not executing?
18. What security risks are associated with running cron jobs as root?
19. How would you verify that an automated backup completed successfully?
20. What is the difference between cron and systemd timers?
21. What is the difference between cron and anacron?
22. Why do cron jobs sometimes fail due to environment variables?
23. How would you prevent overlapping executions of a backup job?
24. How can you monitor scheduled jobs in a production environment?
25. How would you safely test a cron job before deploying it to production?

---

## 📝 19. Quick Revision

| Command / Expression | Purpose |
|----------------------|---------|
| `crontab -e` | Edit cron jobs |
| `crontab -l` | List cron jobs |
| `crontab -r` | Remove all current user's cron jobs |
| `crontab -u user -e` | Edit another user's crontab |
| `* * * * *` | Every minute |
| `*/5 * * * *` | Every 5 minutes |
| `0 0 * * *` | Daily at midnight |
| `0 2 * * 0` | Every Sunday at 2 AM |
| `systemctl status cron` | Check cron service |
| `grep CRON /var/log/syslog` | Check cron logs (Ubuntu/Debian) |
| `bash -n script.sh` | Check shell syntax |
| `chmod +x script.sh` | Add execute permission |

---

## ✅ Chapter 6 Checklist

- [ ] Understand cron and crontab
- [ ] Learn the five cron fields
- [ ] Practice scheduling commands
- [ ] Run shell scripts using cron
- [ ] Practice log redirection
- [ ] Automate backups
- [ ] Automate log cleanup
- [ ] Troubleshoot cron jobs
- [ ] Understand cron security
- [ ] Learn cron vs systemd timers
- [ ] Practice scenario-based interview questions

**Next Chapter:** Linux File Permissions and Ownership 🔐
