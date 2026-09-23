
# 🐧 Linux Files and Directories



---

## 📌 Table of Contents

- [Navigating Directories](#-navigating-directories)
- [Creating Files](#-creating-files)
- [Displaying File Content](#-displaying-file-content)
- [Creating Directories](#-creating-directories)
- [Copying Files and Directories](#-copying-files-and-directories)
- [Moving and Renaming](#-moving-and-renaming)
- [Removing Files and Directories](#-removing-files-and-directories)
- [File Information](#-file-information)
- [Basic File Permissions](#-basic-file-permissions)
- [Practical Example](#-practical-example)

---

## 📂 Navigating Directories

### 1. Print Working Directory

Displays the current working directory.

```bash
pwd
```

### 2. List Files and Directories

```bash
ls
```

### 3. Change Directory

```bash
cd pune
```

### 4. Go to Parent Directory

```bash
cd ..
```

### 5. Go to Home Directory

```bash
cd ~
```

### 6. Go to Previous Directory

```bash
cd -
```

### 7. List Hidden Files

```bash
ls -a
```

### 8. List Files with Detailed Information

```bash
ls -l
```

### 9. Display Human-Readable File Sizes

```bash
ls -lh
```

### 10. List Files Recursively

```bash
ls -R
```

---

## 📝 Creating Files

### 1. Create an Empty File

The `touch` command creates an empty file if it does not exist.

```bash
touch example.txt
```

### 2. Create Multiple Files

```bash
touch file1.txt file2.txt file3.txt
```

### 3. Create a File Using cat

Creates a file and waits for user input.

```bash
cat > file2.txt
```

**Press `Ctrl + D` to save and exit.**

### 4. Create a File Using echo

Creates a file and writes content into it.

```bash
echo "Hello World" > index.html
```

### 5. Append Content to a File

```bash
echo "Welcome to Linux" >> index.html
```

**Note:**

- `>` overwrites existing file content.
- `>>` appends content without removing existing data.

---

## 📄 Displaying File Content

### 1. Display Complete File Content

```bash
cat index.html
```

### 2. Display File Content Page by Page

```bash
less index.html
```

### 3. Display First 10 Lines

```bash
head index.html
```

### 4. Display Last 10 Lines

```bash
tail index.html
```

### 5. Monitor File Changes in Real Time

```bash
tail -f application.log
```

Useful for monitoring application logs.

---

## 📁 Creating Directories

### 1. Create a Directory

```bash
mkdir pune
```

### 2. Create Multiple Directories

```bash
mkdir dir1 dir2 dir3
```

### 3. Create Nested Directories

The `-p` option creates parent directories when needed.

```bash
mkdir -p project/src/components
```

### 4. Create a Directory with Verbose Output

```bash
mkdir -v backup
```

---

## 📋 Copying Files and Directories

### 1. Copy a File

```bash
cp file1.txt file2.txt
```

Copies the content of `file1.txt` into `file2.txt`.

### 2. Copy a File into a Directory

```bash
cp file1.txt pune/
```

### 3. Copy a Directory Recursively

```bash
cp -r pune backup_pune
```

### 4. Copy with Verbose Output

```bash
cp -rv pune backup_pune
```

**Options:**

- `-r` → Copy directories recursively.
- `-v` → Display copied files and directories.

---

## 🚚 Moving and Renaming

The `mv` command is used to move or rename files and directories.

### 1. Move a File to Another Directory

```bash
mv index.html pune/
```

### 2. Rename a File

```bash
mv shreyash.txt sam.txt
```

Renames `shreyash.txt` to `sam.txt`.

### 3. Move a Directory

```bash
mv pune Documents/
```

### 4. Rename a Directory

```bash
mv old_folder new_folder
```

### 5. Move Multiple Files into a Directory

```bash
mv file1.txt file2.txt pune/
```

---

## 🗑️ Removing Files and Directories

### 1. Remove a File

```bash
rm example.txt
```

### 2. Remove Multiple Files

```bash
rm file1.txt file2.txt
```

### 3. Remove an Empty Directory

```bash
rmdir pune
```

### 4. Remove a Directory Recursively

```bash
rm -r pune
```

### 5. Remove with Confirmation

```bash
rm -i example.txt
```

### 6. Remove Recursively with Verbose Output

```bash
rm -rv pune
```

**Options:**

- `-r` → Recursive removal.
- `-v` → Verbose output.
- `-i` → Ask for confirmation.

⚠️ **Warning:** Be careful when using `rm -rf`. It can permanently delete files and directories without confirmation.

---

## 🔍 File Information

### 1. Check File Type

```bash
file example.txt
```

### 2. Check File Size

```bash
du -h example.txt
```

### 3. Display Detailed File Information

```bash
stat example.txt
```

### 4. Check Directory Size

```bash
du -sh pune
```

### 5. Find Files by Name

```bash
find . -name "example.txt"
```

### 6. Find All Text Files

```bash
find . -name "*.txt"
```

---

## 🔐 Basic File Permissions

Linux uses permissions to control access to files and directories.

### 1. View File Permissions

```bash
ls -l example.txt
```

### 2. Change File Permissions

```bash
chmod 644 example.txt
```

### 3. Change Directory Permissions

```bash
chmod 755 pune
```

### Permission Values

| Permission | Value |
|---|---|
| Read (r) | 4 |
| Write (w) | 2 |
| Execute (x) | 1 |

### Example: chmod 755

```bash
chmod 755 script.sh
```

- Owner → Read, Write, Execute
- Group → Read, Execute
- Others → Read, Execute

---

## 🛠️ Practical Example

### Create and Manage a Website Directory

```bash
# Create project directory
mkdir website

# Navigate into the directory
cd website

# Create HTML file
touch index.html

# Add content to HTML file
echo "<h1>Hello DevOps</h1>" > index.html

# Display file content
cat index.html

# Create backup directory
mkdir backup

# Copy HTML file to backup
cp index.html backup/

# List files
ls -l

# Rename HTML file
mv index.html home.html

# Display final directory structure
ls -R
```

---

## 📚 Linux File and Directory Command Cheat Sheet

| Command | Description |
|---|---|
| `pwd` | Show current directory |
| `ls` | List files |
| `cd` | Change directory |
| `touch` | Create an empty file |
| `cat` | Display file content |
| `head` | Display first lines |
| `tail` | Display last lines |
| `mkdir` | Create directory |
| `cp` | Copy files/directories |
| `mv` | Move or rename |
| `rm` | Remove files |
| `rmdir` | Remove empty directory |
| `find` | Search for files |
| `file` | Check file type |
| `stat` | Display file information |
| `du` | Display disk usage |
| `chmod` | Change permissions |

---

## 🎯 Learning Outcome

After practicing these commands, I can:

- Create and manage Linux files.
- Create and navigate directories.
- Copy, move, rename, and delete files.
- View and search file information.
- Understand basic Linux file permissions.
- Perform basic file management tasks in DevOps environments.

---

⭐ **Practice Linux commands regularly to build strong DevOps fundamentals.**
