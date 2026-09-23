
# Archiving and Compression

## 📚 Introduction

**Archiving:** Combining multiple files and directories into a single archive file.

**Compression:** Reducing the size of files to save disk space and bandwidth.

### 🔹 Common Archiving and Compression Tools

| Tool | Purpose |
|------|---------|
| `tar` | Archive files and directories |
| `gzip` | Compress files using gzip |
| `bzip2` | Compress files using bzip2 |
| `xz` | Compress files using xz |
| `zip` | Archive and compress files |
| `unzip` | Extract ZIP archives |

---

## 🔧 TAR Command Options

| Option | Description |
|--------|-------------|
| `c` | Create an archive |
| `v` | Verbose output |
| `f` | Specify archive filename |
| `z` | Use gzip compression |
| `j` | Use bzip2 compression |
| `J` | Use xz compression |
| `x` | Extract an archive |
| `t` | List archive contents |
| `C` | Change directory during extraction |
| `r` | Append files to an archive |
| `--exclude` | Exclude files or directories |

> **Note:** `tar` creates archives. Compression is applied when using options such as `z`, `j`, or `J`.

---

## 🔍 1. Check File and Directory Size

### Check the size of a directory

```bash
du -sh /etc
```

### Check the size of all items

```bash
du -sh *
```

### Check disk space

```bash
df -h
```

### Check the size of a specific file

```bash
du -h backup.tar.gz
```

---

## 📦 2. Create a Simple TAR Archive

### Create an archive without compression

```bash
tar -cvf backup.tar /etc
```

### List archive contents

```bash
tar -tvf backup.tar
```

### Extract TAR archive

```bash
tar -xvf backup.tar
```

### Extract to a specific directory

```bash
tar -xvf backup.tar -C /opt
```

---

## 💾 3. GZIP Compression (.tar.gz)

### Create a compressed archive

```bash
tar -cvzf backup.tar.gz /etc
```

### Extract gzip archive

```bash
tar -xvzf backup.tar.gz -C /opt
```

### List gzip archive contents

```bash
tar -tvzf backup.tar.gz
```

### Compress a single file

```bash
gzip example.txt
```

### Decompress a gzip file

```bash
gunzip example.txt.gz
```

> **Note:** `gzip` replaces the original file with the compressed file by default.

---

## 💾 4. BZIP2 Compression (.tar.bz2)

### Create a compressed archive

```bash
tar -cvjf backup.tar.bz2 /etc
```

### Extract bzip2 archive

```bash
tar -xvjf backup.tar.bz2 -C /opt
```

### Compress a single file

```bash
bzip2 example.txt
```

### Decompress a bzip2 file

```bash
bunzip2 example.txt.bz2
```

---

## 💾 5. XZ Compression (.tar.xz)

### Create a compressed archive

```bash
tar -cvJf backup.tar.xz /etc
```

### Extract xz archive

```bash
tar -xvJf backup.tar.xz -C /opt
```

### Compress a single file

```bash
xz example.txt
```

### Decompress an xz file

```bash
unxz example.txt.xz
```

---

## 🗜️ 6. ZIP and UNZIP

### Create a ZIP archive

```bash
zip -r backup.zip /etc
```

### Extract ZIP archive

```bash
unzip backup.zip -d /opt
```

### List ZIP contents

```bash
unzip -l backup.zip
```

### Compress multiple files

```bash
zip files.zip file1.txt file2.txt
```

### Extract a specific file

```bash
unzip backup.zip file1.txt
```

---

## 📁 7. Exclude Files and Directories

### Exclude a directory from TAR

```bash
tar --exclude="node_modules" -cvzf project.tar.gz project/
```

### Exclude log files

```bash
tar --exclude="*.log" -cvzf backup.tar.gz project/
```

### Exclude multiple directories

```bash
tar --exclude="node_modules" \
    --exclude=".git" \
    -cvzf project.tar.gz project/
```

> **DevOps Use Case:** Exclude unnecessary files such as `node_modules`, `.git`, temporary files, and logs to reduce backup size.

---

## 🔄 8. Compress and Extract Using STDIN/STDOUT

### Compress a directory using gzip

```bash
tar -czf - project/ > project.tar.gz
```

### Extract from a compressed stream

```bash
tar -xzf project.tar.gz
```

### Create a compressed backup using a pipe

```bash
tar -cf - project/ | gzip > project.tar.gz
```

> **DevOps Use Case:** Pipes allow you to connect commands and transfer data between processes without creating an intermediate uncompressed archive.

---

## 🛠️ 9. Backup and Restore Project

### Create a project backup

```bash
tar -czvf project-backup.tar.gz project/
```

### Verify the backup

```bash
tar -tzf project-backup.tar.gz
```

### Create a restore directory

```bash
mkdir restore
```

### Restore the backup

```bash
tar -xzvf project-backup.tar.gz -C restore/
```

### Verify restored files

```bash
ls -la restore/
```

---

## 🔐 10. Preserve Permissions and Ownership

### Create a backup preserving metadata

```bash
sudo tar -czvpf backup.tar.gz /etc
```

### Restore with permissions

```bash
sudo tar -xzvpf backup.tar.gz -C /tmp
```

> **Important:** Root privileges may be required to preserve or restore ownership and permissions. Use caution when restoring system files.

---

## 🧪 11. Check Archive Integrity

### Test a gzip archive

```bash
gzip -t backup.tar.gz
```

### Test a bzip2 archive

```bash
bzip2 -t backup.tar.bz2
```

### Test an xz archive

```bash
xz -t backup.tar.xz
```

### Test a ZIP archive

```bash
unzip -t backup.zip
```

> **DevOps Use Case:** Verify archive integrity before using backups in deployment or disaster recovery.

---

## ⚡ 12. Compression Comparison

| Format | Extension | Command |
|--------|-----------|---------|
| TAR | `.tar` | `tar -cvf` |
| GZIP | `.tar.gz` | `tar -czf` |
| BZIP2 | `.tar.bz2` | `tar -cjf` |
| XZ | `.tar.xz` | `tar -cJf` |
| ZIP | `.zip` | `zip -r` |

### Important Points

- GZIP generally prioritizes speed and is commonly used with TAR.
- BZIP2 and XZ provide alternative compression methods.
- Compression ratio depends on the type and content of files.
- TAR itself does not compress files.
- ZIP combines archiving and compression in a single format.

---

## 🚀 13. Real-World DevOps Use Cases

### 1. Application Backup

```bash
tar -czf app-backup.tar.gz /var/www/app
```

Used to create a compressed backup of application files.

### 2. Log Archiving

```bash
tar -czf logs-backup.tar.gz /var/log/myapp/
```

Used to archive application logs.

### 3. Transfer Files Between Servers

```bash
scp app-backup.tar.gz user@server:/tmp/
```

Transfer a compressed backup to another server using SCP.

### 4. Extract Application Files

```bash
tar -xzf app-backup.tar.gz -C /opt/app/
```

Extract application files into the target directory.

### 5. Archive Old Logs

```bash
tar -czf old-logs.tar.gz /var/log/myapp/*.log
```

Archive selected log files for storage.

> **Security Note:** Avoid including passwords, private keys, or sensitive credentials in backups. Restrict permissions on backup files.

---

## 🎯 14. Practical Hands-On Practice

### Create a practice directory

```bash
mkdir linux-backup
cd linux-backup
```

### Create sample files

```bash
touch file1.txt file2.txt file3.txt
```

### Create a directory

```bash
mkdir project
```

### Create an archive

```bash
tar -cvf project.tar project/
```

### Compress the archive

```bash
gzip project.tar
```

### Extract the archive

```bash
tar -xvzf project.tar.gz
```

### Check the result

```bash
ls -lh
```

---

## 🧠 15. Important Interview Questions

### Basic Questions

1. What is the difference between archiving and compression?
2. What is the use of the `tar` command in Linux?
3. Explain `tar -cvzf`.
4. What is the difference between `.tar` and `.tar.gz`?
5. What is the difference between gzip, bzip2, and xz?
6. How do you extract a `.tar.gz` file?
7. How do you list the contents of a TAR archive?
8. How do you compress a single file using gzip?
9. What is the difference between `zip` and `tar`?
10. How do you check the size of a directory?

### DevOps Scenario-Based Questions

11. How would you back up an application directory in Linux?
12. How do you exclude `node_modules` while creating a backup?
13. How do you restore a compressed backup to a different directory?
14. How do you verify the integrity of a backup archive?
15. Why should you compress log files?
16. How would you transfer a compressed backup to another server?
17. How do you preserve file permissions during backup and restore?
18. What happens if you extract a backup into the wrong directory?
19. How would you reduce the size of a project backup?
20. How would you safely back up application files before deployment?

---

## 📝 Quick Revision

| Command | Purpose |
|---------|---------|
| `tar -cvf` | Create TAR archive |
| `tar -xvf` | Extract TAR archive |
| `tar -tvf` | List TAR contents |
| `tar -czf` | Create gzip archive |
| `tar -xzf` | Extract gzip archive |
| `gzip` | Compress a file |
| `gunzip` | Decompress gzip file |
| `zip -r` | Create ZIP archive |
| `unzip` | Extract ZIP archive |
| `du -sh` | Check directory size |
| `df -h` | Check disk space |

---


  
