
# 🔐 Linux File Permissions

Linux file permissions control who can access files and directories and what actions they can perform.

Permissions are essential for:

- 🛡️ System security
- 👥 User and group access management
- ⚙️ Application configuration
- 🐳 Docker and container environments
- 🔄 CI/CD pipelines
- 🌐 Web server configuration
- 📁 Shared project directories

---

## 📌 Table of Contents

- [Understanding File Permissions](#-understanding-file-permissions)
- [Checking Permissions](#-checking-file-and-directory-permissions)
- [File Types](#-file-types-in-linux)
- [Ownership](#-ownership-in-linux)
- [Permission Types](#-permission-types)
- [Numeric Permissions](#-numeric-permissions)
- [chmod Command](#-chmod-command)
- [chown and chgrp](#-chown-and-chgrp)
- [Directory Permissions](#-directory-permissions)
- [umask](#-umask)
- [Special Permissions](#-special-permissions)
- [ACL](#-access-control-lists-acl)
- [Hard and Soft Links](#-hard-and-soft-links)
- [Recursive Permissions](#-recursive-permissions)
- [Permission Troubleshooting](#-permission-troubleshooting)
- [Practical DevOps Examples](#-practical-devops-examples)
- [Interview Questions](#-interview-questions)
- [Learning Outcome](#-learning-outcome)

---

# 📖 Understanding File Permissions

Linux permissions determine:

1. Who owns a file or directory.
2. Which group is associated with it.
3. What the owner, group, and other users can do.

Linux permissions are divided into three categories:

| Category | Description |
|---|---|
| Owner (u) | User who owns the file |
| Group (g) | Users belonging to the associated group |
| Others (o) | All other users |

### Check Permissions

```bash
ls -l
```

```bash
ls -la
```

```bash
ls -ld dirname
```

```bash
ls -l filename
```

```bash
ls -ltr
```

> 💡 `ll` is commonly configured as an alias for `ls -l`, but it is not a universal command. Use `ls -l` for portability.

---

# 🔍 File Permission Structure

Example:

```text
-rwxr-xr--
```

| Section | Meaning |
|---|---|
| `-` | File type |
| `rwx` | Owner permissions |
| `r-x` | Group permissions |
| `r--` | Others permissions |

### Permission Breakdown

```text
Owner  → rwx
Group  → r-x
Others → r--
```

---

# 📂 File Types in Linux

| Symbol | File Type | Description |
|---|---|---|
| `-` | Regular file | Text, binary, and other ordinary files |
| `d` | Directory | Contains files and directories |
| `l` | Symbolic link | Reference to another file or directory |
| `b` | Block device | Device that transfers data in blocks |
| `c` | Character device | Device that transfers data as a stream of characters |
| `p` | Named pipe (FIFO) | Inter-process communication |
| `s` | Socket | Inter-process communication endpoint |

### Check File Type

```bash
file example.txt
```

---

# 👤 Ownership in Linux

Every file and directory has an owner and group owner.

### Check Ownership

```bash
ls -l example.txt
```

Example:

```text
-rw-r--r-- 1 abhi devops 120 Sep 23 10:00 example.txt
```

- Owner: `abhi`
- Group: `devops`

### Change Owner

```bash
sudo chown abhi example.txt
```

### Change Group Owner

```bash
sudo chgrp devops example.txt
```

### Change Owner and Group

```bash
sudo chown abhi:devops example.txt
```

### Change Ownership Recursively

```bash
sudo chown -R abhi:devops project/
```

> ⚠️ Use recursive ownership changes carefully, especially on system directories.

---

# 🔑 Permission Types

| Permission | Symbol | Numeric Value | File Meaning | Directory Meaning |
|---|---|---|---|---|
| Read | `r` | 4 | Read contents | List directory entries |
| Write | `w` | 2 | Modify contents | Create, delete, or rename entries (subject to other permissions) |
| Execute | `x` | 1 | Execute program | Access/traverse directory |

### Important Directory Permission Notes

- `r` allows listing directory entries.
- `w` allows modifying directory entries when combined with appropriate access.
- `x` allows entering/traversing the directory and accessing items when permitted.
- To delete a file, permissions on the containing directory are generally more important than the file's own write permission.

---

# 🔢 Numeric Permissions

Linux permissions can be represented using numbers.

| Permission | Value |
|---|---|
| Read (`r`) | 4 |
| Write (`w`) | 2 |
| Execute (`x`) | 1 |
| No permission (`-`) | 0 |

### Calculate Permission Value

```text
rwx = 4 + 2 + 1 = 7
rw- = 4 + 2 + 0 = 6
r-x = 4 + 0 + 1 = 5
r-- = 4 + 0 + 0 = 4
```

### Common Permission Examples

| Numeric | Symbolic | Meaning |
|---|---|---|
| 777 | `rwxrwxrwx` | Everyone has all basic permissions |
| 755 | `rwxr-xr-x` | Owner full access, group/others read and execute |
| 750 | `rwxr-x---` | Owner full access, group read and execute |
| 700 | `rwx------` | Owner full access only |
| 644 | `rw-r--r--` | Owner read/write, others read |
| 664 | `rw-rw-r--` | Owner/group read/write, others read |
| 600 | `rw-------` | Owner read/write only |

> ⚠️ Avoid using `777` by default. Apply the minimum permissions required.

---

# 🛠️ chmod Command

The `chmod` command changes file and directory permissions.

## Syntax

```bash
chmod [options] permissions filename
```

---

## 1. Numeric Mode

### File Permission 644

```bash
chmod 644 demo.txt
```

### Directory Permission 755

```bash
chmod 755 demo
```

### Private File Permission 600

```bash
chmod 600 secret.txt
```

---

## 2. Symbolic Mode

### Add Execute Permission for Owner

```bash
chmod u+x script.sh
```

### Remove Write Permission from Others

```bash
chmod o-w demo.txt
```

### Add Read and Execute for Group

```bash
chmod g+rx demo.sh
```

### Set Exact Permissions

```bash
chmod u=rw,g=r,o=r demo.txt
```

### Remove All Permissions for Others

```bash
chmod o= demo.txt
```

### Apply Multiple Changes

```bash
chmod u+x,g+rx,o-r script.sh
```

### Permission Operators

| Operator | Meaning |
|---|---|
| `+` | Add permission |
| `-` | Remove permission |
| `=` | Set exact permission |

### Permission Classes

| Symbol | Meaning |
|---|---|
| `u` | Owner |
| `g` | Group |
| `o` | Others |
| `a` | All (owner, group, others) |

---

# 📁 Directory Permissions

Directory permissions behave differently from file permissions.

| Permission | Directory Behavior |
|---|---|
| `r` | List directory contents |
| `w` | Create, delete, and rename entries when traversal is also permitted |
| `x` | Access/traverse the directory |

### Example

```bash
mkdir project
chmod 755 project
```

### Private Directory

```bash
mkdir private
chmod 700 private
```

Only the owner has basic access.

### Shared Directory

```bash
mkdir shared
chmod 770 shared
```

The owner and group have full basic permissions, while others have no basic permissions.

> 💡 Actual access can also be affected by ACLs, ownership, parent directory permissions, and special permission bits.

---

# 👥 chown and chgrp

## chown

Changes the owner and optionally the group of a file or directory.

```bash
sudo chown username filename
```

```bash
sudo chown username:groupname filename
```

### Recursive Ownership

```bash
sudo chown -R username:groupname directory/
```

## chgrp

Changes the group owner.

```bash
sudo chgrp groupname filename
```

---

# 🎯 umask

`umask` controls which permission bits are cleared when new files and directories are created.

## Check Current umask

```bash
umask
```

```bash
umask -S
```

### Default Maximum Permissions

| Object | Maximum Base Permission |
|---|---|
| Regular file | 666 |
| Directory | 777 |

Regular files do not receive execute permission from the normal base mode.

### Example: umask 022

```bash
umask 022
```

Typical resulting permissions:

| Object | Result |
|---|---|
| File | 644 (`rw-r--r--`) |
| Directory | 755 (`rwxr-xr-x`) |

### Example: umask 002

```bash
umask 002
```

Typical resulting permissions:

| Object | Result |
|---|---|
| File | 664 (`rw-rw-r--`) |
| Directory | 775 (`rwxrwxr-x`) |

> ⚠️ The final permissions can also be affected by application-specified modes and system configuration.

### Set umask Temporarily

```bash
umask 027
```

This affects the current shell and processes launched from it, subject to how the application creates files.

---

# ⭐ Special Permissions

Linux provides three special permission bits:

1. SUID (Set User ID)
2. SGID (Set Group ID)
3. Sticky Bit

---

## 1. SUID (Set User ID)

When set on an executable file, SUID causes the program to run with the effective user ID of the file owner, subject to system security controls.

### Symbolic Mode

```bash
chmod u+s program
```

### Numeric Mode

```bash
chmod 4755 program
```

### Check SUID

```bash
ls -l program
```

Example:

```text
-rwsr-xr-x
```

> ⚠️ SUID can create security risks if used on unsafe executables. Use only when necessary.

---

## 2. SGID (Set Group ID)

### On Executable Files

SGID can cause an executable to run with the effective group ID of the file's group owner, subject to system behavior.

### On Directories

SGID on a directory causes newly created files and subdirectories to inherit the directory's group ownership in typical Linux filesystems.

### Set SGID on Directory

```bash
chmod g+s shared
```

### Numeric Mode

```bash
chmod 2770 shared
```

### Check SGID

```bash
ls -ld shared
```

Example:

```text
drwxrws---
```

### Practical Use

SGID directories are useful for shared team folders where members should inherit a common group.

---

## 3. Sticky Bit

The sticky bit on a directory restricts deletion and renaming of entries to users who have appropriate ownership or administrative privileges.

It is commonly used on shared temporary directories.

### Set Sticky Bit

```bash
chmod +t shared
```

### Numeric Mode

```bash
chmod 1777 shared
```

### Check Sticky Bit

```bash
ls -ld shared
```

Example:

```text
drwxrwxrwt
```

### Common Example

```bash
ls -ld /tmp
```

> 💡 A sticky directory does not automatically make all files safe or private. It mainly controls removal and renaming of entries.

---

# 🔐 Access Control Lists (ACL)

ACLs provide more detailed permissions than the traditional owner/group/others model.

They allow administrators to grant permissions to specific users or groups.

## Check ACL

```bash
getfacl filename
```

## Set ACL for a User

```bash
sudo setfacl -m u:username:rwx filename
```

## Set ACL for a Group

```bash
sudo setfacl -m g:devops:rwx filename
```

## Remove User ACL

```bash
sudo setfacl -x u:username filename
```

## Remove All Extended ACL Entries

```bash
sudo setfacl -b filename
```

## Set Default ACL on a Directory

Default ACLs can be inherited by newly created files and directories.

```bash
sudo setfacl -m d:g:devops:rwx shared/
```

### Check ACL

```bash
getfacl shared/
```

> ⚠️ ACL support depends on the filesystem and system configuration. The effective permissions can be limited by the ACL mask.

---

# 🔗 Hard and Soft Links

Links allow multiple directory entries to reference files or other paths.

## Hard Link

A hard link is another directory entry referring to the same inode (for supported filesystems).

```bash
ln filename hardlink
```

### Properties

- Refers to the same inode as the original file.
- Changes to the file are visible through both links.
- Typically cannot cross filesystems.
- Normally cannot be created for directories by ordinary users.
- The data remains accessible while at least one hard link remains and no process is holding the file open.

## Soft (Symbolic) Link

A symbolic link stores a path to another file or directory.

```bash
ln -s filename softlink
```

### Properties

- Has its own inode.
- Can reference files and directories.
- Can cross filesystems.
- Can become broken if the target path no longer exists.

## Compare Links

| Aspect | Hard Link | Soft Link |
|---|---|---|
| Inode | Same as target file | Separate inode |
| Cross filesystem | Normally no | Yes |
| Directory target | Generally not allowed for ordinary users | Allowed |
| Broken link | No, as long as another link/data exists | Yes, if target path is missing |
| Link count | Increases for the inode | Does not increase target's hard link count |

### Check Inode

```bash
ls -li filename hardlink softlink
```

---

# 🔄 Recursive Permissions

The `-R` option applies an operation recursively to a directory and its contents.

## Recursive chmod

```bash
chmod -R 755 project/
```

> ⚠️ Applying the same permissions to every file and directory can be unsafe. Files and directories often require different permission modes.

## Recommended Approach

Set directory permissions separately from regular files when appropriate.

```bash
find project/ -type d -exec chmod 755 {} \;
```

```bash
find project/ -type f -exec chmod 644 {} \;
```

> ⚠️ Review the target directory before running recursive permission changes.

---

# 🔎 Permission Troubleshooting

## 1. Check File Permissions

```bash
ls -l filename
```

## 2. Check Ownership

```bash
stat filename
```

## 3. Check User Identity

```bash
whoami
```

```bash
id
```

## 4. Check Directory Access

```bash
namei -l /path/to/file
```

Displays permissions along a path.

## 5. Check ACL

```bash
getfacl filename
```

## 6. Test Access as a User

```bash
sudo -u username cat filename
```

This tests reading the file as the specified user, subject to permissions and other security controls.

## 7. Check Parent Directory Permissions

```bash
ls -ld /path
```

> 💡 Permission errors may be caused by missing execute permission on a parent directory, incorrect ownership, ACLs, or security mechanisms such as SELinux or AppArmor.

---

# 🛠️ Practical DevOps Examples

## Example 1: Create a Secure Configuration File

```bash
# Create configuration file
touch app.conf

# Set owner read/write permissions only
chmod 600 app.conf

# Verify permissions
ls -l app.conf
```

Typical permissions:

```text
-rw-------
```

Useful for configuration files containing sensitive settings, although file permissions alone do not guarantee complete secret protection.

---

## Example 2: Make a Deployment Script Executable

```bash
# Create deployment script
touch deploy.sh

# Add execute permission to owner
chmod u+x deploy.sh

# Verify permissions
ls -l deploy.sh
```

Run the script:

```bash
./deploy.sh
```

---

## Example 3: Create a Shared DevOps Directory

```bash
# Create group
sudo groupadd devops

# Create shared directory
sudo mkdir /opt/devops

# Assign group ownership
sudo chown root:devops /opt/devops

# Set group permissions and SGID
sudo chmod 2770 /opt/devops

# Add user to group
sudo usermod -aG devops username

# Verify
ls -ld /opt/devops
```

### Explanation

- Owner: root
- Group: devops
- Owner and group: Full basic permissions
- Others: No basic permissions
- SGID: New entries inherit the directory's group ownership in typical configurations

---

## Example 4: Set File and Directory Permissions Separately

```bash
# Create project directory
mkdir -p project/{scripts,config}

# Create files
touch project/scripts/deploy.sh
touch project/config/app.conf

# Set directory permissions
chmod 755 project
chmod 755 project/scripts
chmod 750 project/config

# Set script permissions
chmod 750 project/scripts/deploy.sh

# Set config permissions
chmod 640 project/config/app.conf

# Verify
ls -lR project
```

---

## Example 5: Use ACL for a Specific Developer

```bash
# Create file
touch app.log

# Set normal permissions
chmod 640 app.log

# Grant read/write permission to a specific user
sudo setfacl -m u:developer:rw app.log

# Check ACL
getfacl app.log
```

> 💡 ACLs are useful when one user needs additional access without changing the basic owner/group permissions for everyone.

---

# 📚 Permission Command Cheat Sheet

| Command | Description |
|---|---|
| `ls -l` | View permissions |
| `ls -ld` | View directory permissions |
| `chmod` | Change permissions |
| `chown` | Change owner and group |
| `chgrp` | Change group owner |
| `umask` | View or set permission mask |
| `getfacl` | View ACL |
| `setfacl` | Modify ACL |
| `find` | Locate files and directories |
| `stat` | Display file metadata |
| `namei -l` | Inspect path permissions |
| `ln` | Create hard link |
| `ln -s` | Create symbolic link |

---

# ❓ Interview Questions

1. What are Linux file permissions?
2. What is the difference between owner, group, and others?
3. Explain `chmod 755` and `chmod 644`.
4. What is the difference between symbolic and numeric permissions?
5. What is the difference between file and directory permissions?
6. What is the purpose of `chown`?
7. What is the difference between `chown` and `chgrp`?
8. What is umask?
9. Why are default file permissions usually based on 666?
10. What are SUID, SGID, and Sticky Bit?
11. What is the purpose of SGID on a directory?
12. What is ACL in Linux?
13. What is the difference between hard and soft links?
14. What happens when a file is deleted but a hard link exists?
15. Why should you avoid using `chmod -R 777`?
16. How do you troubleshoot a permission denied error?
17. How do you create a shared directory for a DevOps team?
18. What is the difference between `r` permission on a file and a directory?
19. How do you grant permissions to a specific user using ACL?
20. How do you make a shell script executable?

---

# 🎯 Learning Outcome

After practicing this chapter, I can:

- Understand Linux file and directory permissions.
- Read and interpret `ls -l` output.
- Manage ownership and group permissions.
- Use chmod in symbolic and numeric modes.
- Understand umask and default permissions.
- Configure SUID, SGID, and Sticky Bit.
- Use ACL for granular access control.
- Understand hard and soft links.
- Troubleshoot common permission errors.
- Apply Linux permissions in DevOps environments.

---

⭐ **Practice file permissions regularly to build strong Linux and DevOps fundamentals.**
