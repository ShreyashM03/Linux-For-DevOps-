
# 📁 Linux Search and Filter Utilities

> Linux search and filter utilities help DevOps engineers find files, analyze logs, troubleshoot issues, and process command output efficiently.

---

## 📚 Table of Contents

- [Overview](#-overview-of-key-utilities)
- [cat Command](#-using-cat)
- [head and tail](#-head-and-tail)
- [grep Command](#-grep-command)
- [sort Command](#-sort-command)
- [uniq Command](#-uniq-command)
- [cut Command](#-cut-command)
- [awk Command](#-awk-command)
- [sed Command](#-sed-command)
- [find Command](#-find-command)
- [locate Command](#-locate-command)
- [xargs Command](#-xargs-command)
- [Pipes and Redirection](#-pipes-and-redirection)
- [DevOps Log Analysis](#-devops-log-analysis)
- [Practical Examples](#-practical-devops-examples)
- [Troubleshooting](#-troubleshooting-examples)
- [Interview Questions](#-important-interview-questions)
- [Quick Revision](#-quick-revision)

---

## 🛠️ Overview of Key Utilities

| Utility | Purpose |
|---|---|
| `cat` | Display file content |
| `head` | Display the beginning of a file |
| `tail` | Display the end of a file |
| `grep` | Search for patterns |
| `sort` | Sort lines |
| `uniq` | Remove or count duplicate adjacent lines |
| `find` | Search files and directories |
| `locate` | Find files using a database |
| `cut` | Extract columns or characters |
| `awk` | Process and analyze structured text |
| `sed` | Search, replace, and transform text |
| `xargs` | Convert input into command arguments |
| `wc` | Count lines, words, and characters |
| `tee` | Display and save command output |

---

# 📖 1. Reading Files

## 🔹 Using `cat`

Display file content:

```bash
cat file.txt
```

Display multiple files:

```bash
cat file1.txt file2.txt
```

Display content with line numbers:

```bash
cat -n file.txt
```

Create a new file:

```bash
cat > file.txt
```

> Press `Ctrl + D` to save the file when using interactive input.

---

## 🔹 Using `head`

Display the first 10 lines:

```bash
head file.txt
```

Display the first 5 lines:

```bash
head -n 5 file.txt
```

Useful for checking the beginning of configuration files and logs.

---

## 🔹 Using `tail`

Display the last 10 lines:

```bash
tail file.txt
```

Display the last 20 lines:

```bash
tail -n 20 file.txt
```

Monitor a log file in real time:

```bash
tail -f /var/log/syslog
```

Monitor an application log:

```bash
tail -f application.log
```

Stop monitoring:

```text
Ctrl + C
```

### `tail -f` vs `tail -F`

| Command | Purpose |
|---|---|
| `tail -f` | Follows a file as it grows |
| `tail -F` | Follows a file even if it is rotated or recreated |

Example:

```bash
tail -F /var/log/application.log
```

---

# 🔍 2. grep Command

The `grep` command searches for matching text or patterns inside files and command output.

## 🔹 Basic Syntax

```bash
grep [options] "pattern" file
```

Search for a word:

```bash
grep "error" application.log
```

Case-insensitive search:

```bash
grep -i "error" application.log
```

Show line numbers:

```bash
grep -n "error" application.log
```

Search recursively inside directories:

```bash
grep -r "TODO" /home/user/project
```

Search only complete words:

```bash
grep -w "failed" application.log
```

Invert the search:

```bash
grep -v "INFO" application.log
```

Count matching lines:

```bash
grep -c "ERROR" application.log
```

Show only matching file names:

```bash
grep -l "error" *.log
```

Use extended regular expressions:

```bash
grep -E "ERROR|CRITICAL" application.log
```

### Common `grep` Options

| Option | Purpose |
|---|---|
| `-i` | Ignore case |
| `-n` | Show line numbers |
| `-r` | Search recursively |
| `-v` | Show non-matching lines |
| `-c` | Count matching lines |
| `-w` | Match complete words |
| `-l` | Display matching file names |
| `-E` | Use extended regular expressions |
| `-A 3` | Show 3 lines after match |
| `-B 3` | Show 3 lines before match |
| `-C 3` | Show 3 lines before and after match |

### Practical Examples

Search for errors and warnings:

```bash
grep -Ei "error|warning" application.log
```

Search for failed SSH login attempts:

```bash
grep -i "failed" /var/log/auth.log
```

Show context around errors:

```bash
grep -n -C 2 "ERROR" application.log
```

> **DevOps Use:** `grep` is commonly used for application log analysis, troubleshooting, and checking service errors.

---

# 🔢 3. sort Command

The `sort` command arranges lines in a specified order.

Sort alphabetically:

```bash
sort file.txt
```

Sort in reverse order:

```bash
sort -r file.txt
```

Sort numerically:

```bash
sort -n numbers.txt
```

Sort and remove duplicate lines:

```bash
sort -u file.txt
```

Sort based on a specific column:

```bash
sort -k2 users.txt
```

Sort by a numeric column:

```bash
sort -k2 -n data.txt
```

Sort by a delimiter-separated field:

```bash
sort -t: -k3 -n /etc/passwd
```

| Option | Purpose |
|---|---|
| `-n` | Numeric sorting |
| `-r` | Reverse sorting |
| `-u` | Remove duplicates |
| `-k` | Sort by a specific field |
| `-t` | Specify a field delimiter |
| `-h` | Human-readable numeric sorting |

---

# 🔁 4. uniq Command

The `uniq` command removes or counts **adjacent duplicate lines**.

Remove adjacent duplicates:

```bash
uniq file.txt
```

Count duplicate lines:

```bash
uniq -c file.txt
```

Display only duplicate lines:

```bash
uniq -d file.txt
```

Display only unique lines:

```bash
uniq -u file.txt
```

Sort before removing duplicates:

```bash
sort file.txt | uniq
```

Sort and count occurrences:

```bash
sort file.txt | uniq -c
```

Display the most frequent entries:

```bash
sort file.txt | uniq -c | sort -nr
```

> **Important:** `uniq` only compares adjacent lines. Use `sort` first when duplicates may be located in different parts of the file.

---

# ✂️ 5. cut Command

The `cut` command extracts selected sections from each line.

## 🔹 Extract Characters

Display the first 5 characters:

```bash
cut -c 1-5 file.txt
```

Display the first character:

```bash
cut -c 1 file.txt
```

## 🔹 Extract Fields

Example file:

```text
101,Shreyash,DevOps
102,Rahul,Developer
103,Amit,Tester
```

Extract the first field:

```bash
cut -d',' -f1 users.csv
```

Extract the second field:

```bash
cut -d',' -f2 users.csv
```

Extract the first and third fields:

```bash
cut -d',' -f1,3 users.csv
```

Extract usernames from `/etc/passwd`:

```bash
cut -d: -f1 /etc/passwd
```

| Option | Purpose |
|---|---|
| `-c` | Select characters |
| `-d` | Set delimiter |
| `-f` | Select fields |

---

# 📊 6. awk Command

`awk` is a powerful text-processing utility used to extract columns, filter data, and perform calculations.

## 🔹 Basic Syntax

```bash
awk 'pattern { action }' file
```

Display the first column:

```bash
awk '{print $1}' file.txt
```

Display the first and second columns:

```bash
awk '{print $1, $2}' file.txt
```

Display the last column:

```bash
awk '{print $NF}' file.txt
```

Display line numbers:

```bash
awk '{print NR, $0}' file.txt
```

Filter rows based on a condition:

```bash
awk '$3 > 80 {print $1, $3}' marks.txt
```

Use a comma as a delimiter:

```bash
awk -F',' '{print $1, $2}' users.csv
```

Print lines containing a specific value:

```bash
awk '$3 == "DevOps" {print $1}' users.txt
```

Calculate the sum of a column:

```bash
awk '{sum += $2} END {print sum}' numbers.txt
```

### Important `awk` Variables

| Variable | Meaning |
|---|---|
| `$0` | Entire current line |
| `$1` | First field |
| `$2` | Second field |
| `$NF` | Last field |
| `NR` | Current record/line number |
| `NF` | Number of fields |
| `FS` | Input field separator |
| `OFS` | Output field separator |

### DevOps Example

Display processes using more than 50% CPU:

```bash
ps -eo pid,comm,%cpu --no-headers | awk '$3 > 50'
```

> The command displays processes whose CPU usage is greater than 50% at the time of sampling.

---

# 📝 7. sed Command

`sed` stands for **Stream Editor**. It is used to search, replace, delete, and modify text streams.

## 🔹 Display Specific Lines

Display the first 5 lines:

```bash
sed -n '1,5p' file.txt
```

Display line 10:

```bash
sed -n '10p' file.txt
```

## 🔹 Search and Replace

Replace the first occurrence on each line:

```bash
sed 's/old/new/' file.txt
```

Replace all occurrences on each line:

```bash
sed 's/old/new/g' file.txt
```

Replace text and save changes directly:

```bash
sed -i 's/old/new/g' file.txt
```

> **Warning:** Use `sed -i` carefully because it modifies the original file. Create a backup when necessary.

Create a backup while editing:

```bash
sed -i.bak 's/old/new/g' file.txt
```

## 🔹 Delete Lines

Delete line 3:

```bash
sed '3d' file.txt
```

Delete empty lines:

```bash
sed '/^$/d' file.txt
```

Delete lines containing a pattern:

```bash
sed '/DEBUG/d' application.log
```

Comment lines beginning with `#` are ignored in many configuration files. Always understand the file format before modifying it.

### Common `sed` Symbols

| Symbol | Purpose |
|---|---|
| `s` | Substitute |
| `g` | Replace all matches on a line |
| `d` | Delete |
| `p` | Print |
| `-n` | Suppress automatic output |
| `-i` | Edit file in place |

---

# 🔎 8. find Command

The `find` command searches files and directories recursively based on conditions such as name, type, size, ownership, and modification time.

## 🔹 Basic Syntax

```bash
find <path> <options> <expression>
```

Search by name:

```bash
find /home/user -name "file.txt"
```

Case-insensitive name search:

```bash
find /home/user -iname "file.txt"
```

Find only files:

```bash
find /var/log -type f
```

Find only directories:

```bash
find /var/log -type d
```

Find files larger than 100 MB:

```bash
find /var -type f -size +100M
```

Find files smaller than 1 KB:

```bash
find /home -type f -size -1k
```

Find files modified in the last 7 days:

```bash
find /home -type f -mtime -7
```

Find files modified more than 30 days ago:

```bash
find /home -type f -mtime +30
```

Find files owned by a user:

```bash
find /home -user username
```

Find files with specific permissions:

```bash
find /path -type f -perm 644
```

Limit search depth:

```bash
find /home/user -maxdepth 2 -type f
```

Exclude a directory:

```bash
find /project -path "/project/.git" -prune -o -type f -print
```

## 🔹 Execute Commands with `find`

Display detailed information:

```bash
find /var/log -type f -exec ls -lh {} \;
```

Search and print file names:

```bash
find /var/log -type f -name "*.log" -print
```

> Avoid using destructive commands such as `rm` with `find` until you have verified the matching files.

---

# 📍 9. locate Command

The `locate` command searches for files using a prebuilt database.

Search for a file:

```bash
locate nginx.conf
```

Search for all files containing a name:

```bash
locate "*.log"
```

Update the locate database on systems that use `updatedb`:

```bash
sudo updatedb
```

### `find` vs `locate`

| Feature | `find` | `locate` |
|---|---|---|
| Search method | Searches the filesystem | Searches a database |
| Speed | Can be slower | Usually faster |
| Real-time results | Yes | Database may be outdated |
| Advanced filters | Many | Limited |
| Permissions | Respects filesystem access | Depends on database |

---

# ⚙️ 10. xargs Command

`xargs` takes input from standard input and uses it as arguments for another command.

Display files:

```bash
printf "file1.txt\nfile2.txt\n" | xargs ls -l
```

Search for a pattern in multiple files:

```bash
find . -name "*.log" -print0 | xargs -0 grep -n "ERROR"
```

Remove empty files after reviewing the results:

```bash
find . -type f -empty -print
```

Run a command for each input item:

```bash
printf "one\ntwo\nthree\n" | xargs -n1 echo
```

> **Safety:** Use `-print0` with `find` and `-0` with `xargs` to correctly handle filenames containing spaces or special characters.

---

# 🔢 11. wc Command

The `wc` command counts lines, words, bytes, and characters.

Count lines:

```bash
wc -l file.txt
```

Count words:

```bash
wc -w file.txt
```

Count bytes:

```bash
wc -c file.txt
```

Count characters:

```bash
wc -m file.txt
```

Count the number of errors in a log:

```bash
grep -i "error" application.log | wc -l
```

---

# 🔗 12. Pipes and Redirection

## 🔹 Pipe (`|`)

A pipe sends the output of one command as input to another command.

```bash
cat application.log | grep "ERROR"
```

A more efficient approach:

```bash
grep "ERROR" application.log
```

Count errors:

```bash
grep -i "error" application.log | wc -l
```

Find the top repeated IP addresses:

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head
```

## 🔹 Output Redirection

Overwrite a file:

```bash
ls -l > output.txt
```

Append to a file:

```bash
ls -l >> output.txt
```

Redirect errors:

```bash
command 2> errors.log
```

Redirect output and errors:

```bash
command > output.log 2>&1
```

## 🔹 tee Command

Display output and save it to a file:

```bash
ls -l | tee output.txt
```

Append output using `tee`:

```bash
ls -l | tee -a output.txt
```

---

# 📈 13. DevOps Log Analysis

Logs are important for troubleshooting application failures, deployment problems, and server issues.

## 🔹 Find Errors

```bash
grep -i "error" application.log
```

## 🔹 Count Errors

```bash
grep -i "error" application.log | wc -l
```

## 🔹 Monitor Logs in Real Time

```bash
tail -f application.log
```

## 🔹 Find HTTP 500 Errors

For a log format where the HTTP status code is the ninth field:

```bash
awk '$9 == 500 {print}' access.log
```

> Log formats differ. Check your application's log format before relying on a specific field number.

## 🔹 Find the Most Frequent IP Addresses

For a standard access log where the IP address is the first field:

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -10
```

## 🔹 Search Multiple Error Patterns

```bash
grep -Ein "error|failed|critical|exception" application.log
```

## 🔹 Find Recent Errors

```bash
tail -n 1000 application.log | grep -i "error"
```

---

# 🧪 14. Practical DevOps Examples

## Example 1: Find Large Log Files

```bash
find /var/log -type f -size +100M -exec ls -lh {} \;
```

**Use case:** Identify log files consuming disk space.

---

## Example 2: Find Recently Modified Configuration Files

```bash
find /etc -type f -mmin -60 2>/dev/null
```

**Use case:** Investigate recently changed configuration files.

---

## Example 3: Find Running Docker-Related Processes

```bash
ps aux | grep -i docker | grep -v grep
```

**Use case:** Quickly inspect Docker-related processes.

---

## Example 4: Search Failed Services in Logs

```bash
grep -iE "failed|failure|error" /var/log/syslog
```

**Use case:** Investigate service or system errors.

---

## Example 5: Find Files with Specific Extensions

Find shell scripts:

```bash
find . -type f -name "*.sh"
```

Find YAML files:

```bash
find . -type f \( -name "*.yml" -o -name "*.yaml" \)
```

Find Dockerfiles:

```bash
find . -type f -iname "Dockerfile*"
```

---

## Example 6: Count Unique Error Messages

```bash
grep -i "error" application.log | sort | uniq -c | sort -nr
```

**Use case:** Identify repeated error messages during troubleshooting.

---

## Example 7: Search for TODO Comments in a Project

```bash
grep -RIn --exclude-dir=.git "TODO" .
```

**Use case:** Find pending tasks in source code.

---

# 🛠️ 15. Troubleshooting Examples

## Problem 1: Application Is Generating Errors

Check the latest logs:

```bash
tail -n 100 application.log
```

Search for errors:

```bash
grep -iE "error|exception|failed" application.log
```

Monitor new errors:

```bash
tail -f application.log | grep -i "error"
```

---

## Problem 2: Disk Space Is Running Out

Check disk usage:

```bash
df -h
```

Find large files:

```bash
find /var -type f -size +500M -exec ls -lh {} \; 2>/dev/null
```

Review log directories:

```bash
du -sh /var/log/*
```

---

## Problem 3: Find a Configuration File

Search by name:

```bash
find /etc -type f -name "nginx.conf" 2>/dev/null
```

Search for a configuration setting:

```bash
grep -RIn "server_name" /etc/nginx 2>/dev/null
```

---

## Problem 4: Identify Frequent HTTP Status Codes

Example for a common access log format:

```bash
awk '{print $9}' access.log | sort | uniq -c | sort -nr
```

> Verify the position of the HTTP status code in your actual log format.

---

# 🚀 16. DevOps Use Cases

| Task | Useful Commands |
|---|---|
| Analyze application logs | `grep`, `awk`, `tail` |
| Find large files | `find`, `du` |
| Search configuration files | `find`, `grep` |
| Identify repeated errors | `sort`, `uniq`, `grep` |
| Extract log columns | `cut`, `awk` |
| Modify configuration values | `sed` |
| Monitor real-time logs | `tail -f` |
| Analyze access logs | `awk`, `sort`, `uniq` |
| Process multiple files | `find`, `xargs` |
| Investigate disk usage | `find`, `du`, `sort` |

---

# 🎯 17. Hands-on Practice Tasks

Try these tasks on your Linux machine.

- [ ] Create a text file and display it using `cat`.
- [ ] Display the first 5 lines using `head`.
- [ ] Monitor a log using `tail -f`.
- [ ] Search for `ERROR` using `grep`.
- [ ] Perform a case-insensitive search.
- [ ] Sort a file alphabetically and numerically.
- [ ] Remove duplicate lines using `sort` and `uniq`.
- [ ] Extract columns using `cut`.
- [ ] Extract fields using `awk`.
- [ ] Replace text using `sed`.
- [ ] Find files larger than 100 MB.
- [ ] Find files modified in the last 24 hours.
- [ ] Count errors in an application log.
- [ ] Find the top 10 repeated IP addresses.
- [ ] Use `find` with `xargs` to search multiple files.

---

# ❓ 18. Important Interview Questions

## Basic Questions

1. What is the purpose of the `grep` command?
2. What is the difference between `grep` and `find`?
3. What is the difference between `cat`, `head`, and `tail`?
4. How do you search for a word without considering case?
5. How do you display line numbers using `grep`?
6. What is the difference between `sort` and `uniq`?
7. Why should you use `sort` before `uniq`?
8. What is the purpose of the `wc` command?
9. What is the difference between `>` and `>>`?
10. What is a pipe in Linux?

## Intermediate Questions

11. How do you search recursively using `grep`?
12. How do you find files larger than 100 MB?
13. How do you find files modified in the last 7 days?
14. What is the difference between `find` and `locate`?
15. How do you extract a particular column using `cut`?
16. What is the difference between `awk` and `cut`?
17. What is the purpose of `sed`?
18. How do you replace a word in a file using `sed`?
19. What is the difference between `sed` and `grep`?
20. What is the purpose of `xargs`?
21. Why are `find -print0` and `xargs -0` used together?
22. How do you count the number of errors in a log file?
23. How do you display only duplicate lines using `uniq`?
24. How do you find the top 10 repeated lines in a file?
25. How do you monitor a log file in real time?

## DevOps Scenario-Based Questions

26. An application is failing. How will you search its logs?
27. How will you find large files if disk space is low?
28. How will you identify the most frequent IP address in an access log?
29. How will you search for HTTP 500 errors?
30. How will you find recently modified configuration files?
31. How will you search for `ERROR`, `WARNING`, and `CRITICAL` together?
32. How will you count the number of failed login attempts?
33. How will you find all shell scripts in a project?
34. How can you search for a configuration parameter across a directory?
35. How would you safely replace a configuration value using `sed`?
36. How can you exclude the `.git` directory while searching?
37. How do you process filenames containing spaces safely?
38. How would you identify repeated error messages from a log?
39. How can you display the last 100 lines containing errors?
40. How can search and filter utilities help during CI/CD troubleshooting?

---

# ⚡ 19. Quick Revision

| Command | Example | Purpose |
|---|---|---|
| `cat` | `cat file.txt` | Display file content |
| `head` | `head -n 5 file.txt` | First 5 lines |
| `tail` | `tail -f app.log` | Follow logs |
| `grep` | `grep -i error app.log` | Search patterns |
| `sort` | `sort file.txt` | Sort lines |
| `uniq` | `uniq -c file.txt` | Count adjacent duplicates |
| `find` | `find . -name "*.log"` | Search files |
| `locate` | `locate nginx.conf` | Database-based search |
| `cut` | `cut -d: -f1 /etc/passwd` | Extract fields |
| `awk` | `awk '{print $1}' file` | Process columns |
| `sed` | `sed 's/old/new/g' file` | Replace text |
| `xargs` | `find . -print0 \| xargs -0 grep error` | Process input |
| `wc` | `wc -l file.txt` | Count lines |
| `tee` | `command \| tee output.txt` | Display and save output |

---

## 🧠 Important Concept to Remember

```text
grep  → Search text
find  → Search files and directories
sort  → Arrange lines
uniq  → Remove or count adjacent duplicates
cut   → Extract fields
awk   → Analyze and process data
sed   → Modify or transform text
xargs → Pass input as command arguments
```

---

## 🏆 Final DevOps Practice Challenge

Analyze an application access log and complete the following:

```bash
# 1. Count total lines
wc -l access.log

# 2. Find HTTP 500 responses
awk '$9 == 500 {print}' access.log

# 3. Count HTTP 500 responses
awk '$9 == 500 {count++} END {print count}' access.log

# 4. Find the top 10 IP addresses
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -10

# 5. Search for errors
grep -Ein "error|failed|exception" access.log

# 6. Monitor new log entries
tail -f access.log
```

> **Goal:** Practice searching, filtering, extracting, counting, and analyzing data using Linux commands.

---

## 📌 Key Takeaway

Linux search and filter utilities are essential for DevOps engineers because they help with:

- Log monitoring
- Application troubleshooting
- Disk space investigation
- Configuration management
- CI/CD debugging
- Server administration
- Performance analysis
- Automation and scripting
