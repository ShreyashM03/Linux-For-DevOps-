
# 👥 Linux User and Group Management

Linux User and Group Management is an essential topic for DevOps engineers and Linux administrators.

It helps manage system access, permissions, security, and user privileges in Linux environments.

DevOps engineers use user and group management to:

- 👤 Create and manage users
- 👥 Organize users into groups
- 🔐 Control access to files and directories
- ⚙️ Manage system and service accounts
- 🛡️ Configure administrative privileges
- 🐳 Manage permissions for applications and services

---

## 📌 Table of Contents

- [Types of Users](#-types-of-users-in-linux)
- [Root User](#1--root-user)
- [System Users](#2--system-users)
- [Regular Users](#3--regular-users)
- [User IDs and Group IDs](#-user-id-and-group-id)
- [User Management Commands](#-user-management-commands)
- [Important User Files](#-important-user-configuration-files)
- [Password Management](#-password-management)
- [User Modification](#-modify-an-existing-user)
- [User Locking and Unlocking](#-user-locking-and-unlocking)
- [Group Management](#-group-management)
- [Primary and Secondary Groups](#-primary-and-secondary-groups)
- [Sudo and Administrative Privileges](#-sudo-and-administrative-privileges)
- [File Ownership](#-file-ownership)
- [Practical DevOps Examples](#-practical-devops-examples)
- [Learning Outcome](#-learning-outcome)

---

# 👤 Types of Users in Linux

Linux users can be categorized into the following types:

## 1. 🔴 Root User

The root user is the superuser in Linux with unrestricted administrative privileges.

- User ID (UID) is `0`.
- Can manage users and groups.
- Can modify system configuration.
- Can access and manage most files.
- Can install and remove software.
- Can change file ownership and permissions.

### Common Commands

```bash
# Switch to a root login shell
sudo -i
```

```bash
# Execute a command with administrative privileges
sudo <command>
```

Example:

```bash
sudo apt update
```

> ⚠️ **Security Tip:** Avoid performing daily tasks as root. Use a regular user with `sudo` privileges when administrative access is required.

---

## 2. 🔵 System Users

System users are accounts commonly used to run system services and applications.

Examples include accounts created for services such as web servers and other daemons.

- Often used by applications or background services.
- Usually have restricted privileges.
- Frequently configured with a non-interactive shell.
- UID ranges are distribution-dependent.

Example:

```bash
# Create a system user
sudo useradd --system nginxuser
```

Check the account:

```bash
getent passwd nginxuser
```

> 💡 System users are not automatically secure just because they are system accounts. Their permissions and configuration must be managed properly.

---

## 3. 🟢 Regular Users

Regular users are accounts created for human users and everyday operations.

- Usually have a home directory.
- Have their own UID and primary group.
- Access is controlled through permissions.
- May receive administrative privileges through `sudo`.

### Create a Regular User

```bash
sudo adduser tony
```

On Debian/Ubuntu systems, `adduser` provides an interactive account creation process.

### Using useradd

```bash
sudo useradd -m username
```

- `-m` → Creates a home directory if needed.

Set a password:

```bash
sudo passwd username
```

---

# 🆔 User ID and Group ID

## UID (User ID)

UID is a unique numerical identifier assigned to a Linux user.

Example:

```text
Username: tony
UID: 1001
```

## GID (Group ID)

GID is a numerical identifier assigned to a group.

Each user has a primary group.

### Check UID and GID

```bash
id username
```

Example output:

```text
uid=1001(tony) gid=1001(tony) groups=1001(tony)
```

### Important Notes

- Root user has UID `0`.
- System and regular user UID ranges depend on Linux distribution and configuration.
- UID and GID are used by Linux to identify ownership.
- Numeric IDs are more important to the system than usernames.

---

# 🛠️ User Management Commands

## 1. Create a User

```bash
sudo adduser tony
```

Or:

```bash
sudo useradd -m tony
```

## 2. Set or Change Password

```bash
sudo passwd tony
```

## 3. Display Current User

```bash
whoami
```

## 4. Display User ID and Groups

```bash
id tony
```

## 5. Display Current Login Information

```bash
who
```

```bash
w
```

## 6. List Users

```bash
cat /etc/passwd
```

Or:

```bash
getent passwd
```

## 7. Find a Specific User

```bash
getent passwd tony
```

## 8. Delete a User

```bash
sudo userdel tony
```

## 9. Delete a User and Their Home Directory

```bash
sudo userdel -r tony
```

> ⚠️ **Warning:** Make sure important files and ownership are reviewed before deleting a user and their home directory.

---

# 📄 Important User Configuration Files

Linux stores user and group information in important configuration files.

| File | Purpose |
|---|---|
| `/etc/passwd` | User account information |
| `/etc/shadow` | Password hashes and password aging data |
| `/etc/group` | Group information and memberships |
| `/etc/gshadow` | Group administration and security information |
| `/etc/login.defs` | Default login and account settings |
| `/etc/skel` | Files copied to new user home directories |

---

## 📁 /etc/passwd

The `/etc/passwd` file stores basic information about user accounts.

### View the File

```bash
cat /etc/passwd
```

### Example Entry

```text
steve:x:1001:1001:Steve User:/home/steve:/bin/bash
```

### Fields in /etc/passwd

| Field | Description |
|---|---|
| 1. Username | Name of the user |
| 2. Password Placeholder | Usually `x`, indicating the hash is stored in `/etc/shadow` |
| 3. UID | User ID |
| 4. GID | Primary Group ID |
| 5. GECOS | User information or comment |
| 6. Home Directory | User's home directory |
| 7. Login Shell | Shell assigned to the user |

---

## 📁 /etc/shadow

The `/etc/shadow` file stores password hashes and password aging information.

```bash
sudo cat /etc/shadow
```

> ⚠️ **Security:** Access to this file is restricted because it contains sensitive authentication data.

### Common Fields

| Field | Description |
|---|---|
| Username | User account name |
| Password Hash | Stored password hash or account status marker |
| Last Change | Date of last password change |
| Minimum Days | Minimum days between password changes |
| Maximum Days | Maximum password age |
| Warning Days | Warning period before expiration |
| Inactive Days | Days after expiration before account is disabled |
| Expiry Date | Account expiration date |
| Reserved | Reserved field |

---

# 🔑 Password Management

## 1. Change User Password

```bash
sudo passwd username
```

## 2. Check Password Aging Information

```bash
sudo chage -l username
```

## 3. Set Maximum Password Age

```bash
sudo chage -M 30 username
```

Sets the maximum password age to 30 days.

## 4. Set Minimum Password Age

```bash
sudo chage -m 7 username
```

Sets the minimum number of days between password changes to 7.

## 5. Set Password Expiry Warning

```bash
sudo chage -W 5 username
```

Sets a 5-day warning period.

## 6. Set Account Inactivity Period

```bash
sudo chage -I 10 username
```

Sets the inactive period after password expiration to 10 days.

## 7. Set Account Expiration Date

```bash
sudo chage -E 2026-12-31 username
```

Sets the account expiration date.

> 💡 Password aging settings should follow your organization's security requirements.

---

# ✏️ Modify an Existing User

The `usermod` command is used to modify user account settings.

## 1. Change User's UID

```bash
sudo usermod -u 3000 tony
```

Changes the UID to `3000`.

> ⚠️ Existing file ownership and related configuration must be reviewed when changing a UID.

## 2. Change User's Login Shell

```bash
sudo usermod -s /bin/bash tony
```

## 3. Assign a No-Login Shell

```bash
sudo usermod -s /usr/sbin/nologin steve
```

The exact path can vary by distribution.

## 4. Add a Comment

```bash
sudo usermod -c "Development Team" bruce
```

## 5. Change Home Directory

```bash
sudo usermod -d /home/newhome -m tony
```

- `-d` → Sets the home directory.
- `-m` → Moves existing home directory content when applicable.

## 6. Rename a User

```bash
sudo usermod -l newname oldname
```

> ⚠️ Renaming a user may require reviewing the home directory, ownership, and related configuration.

---

# 🔒 User Locking and Unlocking

Account locking is useful when temporarily disabling password-based login.

## 1. Lock a User's Password

```bash
sudo usermod -L natasha
```

## 2. Unlock a User's Password

```bash
sudo usermod -U natasha
```

## 3. Check Password Status

```bash
sudo passwd -S natasha
```

> 💡 Locking a password is not the same as completely disabling every possible authentication method. Review SSH keys, account status, and other login mechanisms when disabling access.

---

# 👥 Group Management

Groups are used to organize users and manage access to files, directories, and resources.

### Benefits of Groups

- Simplifies permission management.
- Allows multiple users to share access.
- Reduces the need to assign permissions individually.
- Helps manage access for teams and applications.

---

## 1. Create a Group

```bash
sudo groupadd avengers
```

## 2. View Group Information

```bash
getent group avengers
```

## 3. List All Groups

```bash
cat /etc/group
```

Or:

```bash
getent group
```

## 4. Delete a Group

```bash
sudo groupdel avengers
```

> ⚠️ A group cannot generally be deleted if it is the primary group of an existing user.

## 5. Change Group ID

```bash
sudo groupmod -g 3000 avengers
```

Changes the group's GID.

---

# 📄 Important Group Configuration Files

## /etc/group

Stores group information.

### Example

```text
avengers:x:2000:steve,thor
```

### Fields

| Field | Description |
|---|---|
| Group Name | Name of the group |
| Password Placeholder | Usually `x` |
| GID | Group ID |
| Members | Comma-separated supplementary group members |

## /etc/gshadow

Stores group security and administration information.

```bash
sudo cat /etc/gshadow
```

### Fields

| Field | Description |
|---|---|
| Group Name | Name of the group |
| Password | Group password hash or status |
| Administrators | Group administrators |
| Members | Group members |

> ⚠️ Group passwords are an older Linux feature and are not a replacement for properly configured user permissions.

---

# 🔗 Primary and Secondary Groups

## Primary Group

The primary group is the main group associated with a user account.

It is identified by the user's GID in `/etc/passwd`.

## Secondary (Supplementary) Groups

These are additional groups a user belongs to.

They allow users to access resources shared with multiple groups.

### Check Group Membership

```bash
groups username
```

Or:

```bash
id username
```

---

## ➕ Add a User to a Group

### Using usermod

```bash
sudo usermod -aG avengers natasha
```

- `-a` → Append.
- `-G` → Supplementary groups.

> ⚠️ Always use `-a` with `-G` when adding a user to an existing group list. Omitting `-a` can replace supplementary group memberships.

### Using gpasswd

```bash
sudo gpasswd -a steve avengers
```

## ➖ Remove a User from a Group

```bash
sudo gpasswd -d steve avengers
```

## 👥 Add Multiple Users to a Group

```bash
sudo gpasswd -M steve,thor,bruce avengers
```

> ⚠️ `gpasswd -M` sets the group membership list and can remove existing members who are not included. Use it carefully.

## 🛠️ Set a User's Primary Group

```bash
sudo usermod -g avengers natasha
```

- `-g` → Sets the primary group.

## 🔄 Switch to a Different Group

```bash
newgrp avengers
```

Starts a new shell with the selected group as the effective group.

---

# 🛡️ Sudo and Administrative Privileges

`sudo` allows an authorized user to execute commands with another user's privileges, commonly root.

## 1. Run a Command as Root

```bash
sudo apt update
```

## 2. Open a Root Login Shell

```bash
sudo -i
```

## 3. Check Sudo Permissions

```bash
sudo -l
```

## 4. Edit Sudo Configuration Safely

```bash
sudo visudo
```

`visudo` validates the sudoers configuration before saving, helping prevent syntax errors.

## 5. Add a User to the Sudo Group (Ubuntu/Debian)

```bash
sudo usermod -aG sudo username
```

> 💡 On some distributions, administrative access is configured through a different group, such as `wheel`.

---

# 🔐 File Ownership

Linux files and directories have an owner and an associated group.

Ownership is important when configuring applications, scripts, and services.

## 1. Check File Ownership

```bash
ls -l example.txt
```

Example:

```text
-rw-r--r-- 1 tony developers 120 Sep 23 10:00 example.txt
```

- Owner: `tony`
- Group: `developers`

## 2. Change File Owner

```bash
sudo chown steve example.txt
```

## 3. Change File Group

```bash
sudo chgrp developers example.txt
```

## 4. Change Owner and Group

```bash
sudo chown steve:developers example.txt
```

## 5. Change Directory Ownership Recursively

```bash
sudo chown -R steve:developers project/
```

> ⚠️ Use recursive ownership changes carefully. Incorrect ownership can break system services and application access.

---

# 🛠️ Practical DevOps Examples

## Example 1: Create a Developer User

```bash
# Create user
sudo adduser developer

# Check user information
id developer

# View home directory
ls -la /home/developer
```

---

## Example 2: Create a DevOps Team Group

```bash
# Create group
sudo groupadd devops

# Create users
sudo adduser dev1
sudo adduser dev2

# Add users to devops group
sudo usermod -aG devops dev1
sudo usermod -aG devops dev2

# Verify group membership
getent group devops
```

---

## Example 3: Shared Project Directory

```bash
# Create project directory
sudo mkdir /opt/devops-project

# Create group
sudo groupadd projectteam

# Change group ownership
sudo chown root:projectteam /opt/devops-project

# Allow owner and group to read/write/execute
sudo chmod 770 /opt/devops-project

# Add a user to the group
sudo usermod -aG projectteam dev1

# Verify permissions
ls -ld /opt/devops-project
```

> 💡 The group members can access the directory according to its permissions. A new login session may be required for a user's updated group membership to appear.

---

## Example 4: Create a Service User

```bash
# Create a system user
sudo useradd --system --no-create-home --shell /usr/sbin/nologin appuser

# Check account information
getent passwd appuser
```

This example creates an account intended for running an application without an interactive login shell.

---

## Example 5: Lock a User Account

```bash
# Lock the user's password
sudo usermod -L developer

# Check password status
sudo passwd -S developer

# Unlock the user's password
sudo usermod -U developer
```

---

# 📚 User and Group Command Cheat Sheet

| Command | Description |
|---|---|
| `whoami` | Display current user |
| `id` | Display UID, GID, and group memberships |
| `adduser` | Create a user (Debian/Ubuntu utility) |
| `useradd` | Create a user |
| `usermod` | Modify a user |
| `userdel` | Delete a user |
| `passwd` | Set or change password |
| `chage` | Manage password aging |
| `groupadd` | Create a group |
| `groupmod` | Modify a group |
| `groupdel` | Delete a group |
| `gpasswd` | Manage group membership |
| `groups` | Display group memberships |
| `newgrp` | Start a shell with a different group |
| `chown` | Change file ownership |
| `chgrp` | Change file group |
| `sudo` | Execute authorized commands with elevated privileges |
| `visudo` | Safely edit sudoers configuration |
| `getent` | Query system databases |
| `cat /etc/passwd` | View user account records |
| `cat /etc/group` | View group records |

---

# 🎯 Learning Outcome

After practicing this chapter, I can:

- Understand root, system, and regular users.
- Create, modify, and delete Linux users.
- Understand UID and GID.
- Create and manage Linux groups.
- Add and remove users from groups.
- Understand primary and supplementary groups.
- Manage passwords and account expiration.
- Lock and unlock user accounts.
- Configure basic administrative privileges using sudo.
- Manage file ownership for shared project directories.
- Apply Linux user and group management in DevOps environments.

---

# ❓ Interview Questions to Practice

1. What is the difference between root and a regular user?
2. What is UID and GID in Linux?
3. What is the difference between `useradd` and `adduser`?
4. What is the difference between primary and secondary groups?
5. What is the use of `usermod -aG`?
6. What happens if you use `usermod -G` without `-a`?
7. What is the difference between `/etc/passwd` and `/etc/shadow`?
8. What is the purpose of `/etc/group`?
9. How do you lock and unlock a Linux user?
10. How do you create a system user?
11. What is the purpose of `sudo`?
12. What is the difference between `chown` and `chmod`?
13. How do you create a shared directory for a DevOps team?
14. Why should applications avoid running as root?
15. How do you check a user's group membership?

---

⭐ **Practice Linux user and group management regularly to build strong DevOps fundamentals.**
