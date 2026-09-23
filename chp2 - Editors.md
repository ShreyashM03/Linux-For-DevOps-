
# 📝 Linux Text Editors for DevOps

Linux text editors are essential tools for creating, editing, and managing configuration files, shell scripts, logs, and application settings.

DevOps engineers frequently use text editors to modify:

- ⚙️ Configuration files
- 🐚 Shell scripts
- 🐳 Dockerfiles
- 🔄 CI/CD pipeline files
- ☸️ Kubernetes YAML files
- 🌐 Web server configurations
- 📄 Application and system files

---

## 📌 Table of Contents

- [Types of Linux Editors](#-types-of-linux-editors)
- [1. Vi Editor](#1--vi-editor)
- [2. Vim Editor](#2--vim-editor)
- [3. Nano Editor](#3--nano-editor)
- [4. Emacs Editor](#4--emacs-editor)
- [5. Gedit Editor](#5--gedit-editor)
- [6. Sed Stream Editor](#6--sed-stream-editor)
- [Vim Modes](#-vim-modes)
- [Vim Command Cheat Sheet](#-vim-command-cheat-sheet)
- [Practical DevOps Examples](#-practical-devops-examples)
- [Vim vs Nano](#-vim-vs-nano)
- [Learning Outcome](#-learning-outcome)

---

# 📚 Types of Linux Editors

Linux editors can be broadly categorized as follows:

| Editor | Type | Interface | Common Use |
|---|---|---|---|
| Vi | Terminal-based | CLI | Basic text editing |
| Vim | Terminal-based | CLI | Advanced text editing |
| Nano | Terminal-based | CLI | Beginner-friendly editing |
| Emacs | Terminal/GUI | CLI/GUI | Advanced editing and development |
| Gedit | Graphical | GUI | Editing text using a desktop |
| Sed | Stream editor | CLI | Automated text processing |

> 💡 **DevOps Tip:** Vim, Nano, and Sed are especially useful when working with Linux servers and automation.

---

# 1️⃣ Vi Editor

## 📌 What is Vi?

Vi is a terminal-based text editor available on many Unix and Linux systems. It uses different modes for editing and executing commands.

### Open or Create a File

```bash
vi example.txt
```

### Vi Modes

| Mode | Description |
|---|---|
| Command Mode | Navigate and perform editing operations |
| Insert Mode | Insert or modify text |
| Last-Line Mode | Execute commands such as save and quit |

### Basic Vi Commands

```text
i   → Insert text
Esc → Return to Command Mode
:w  → Save
:q  → Quit
:wq → Save and quit
:q! → Quit without saving
```

> ⚠️ Vi and Vim share many commands, but Vim provides additional features and improvements.

---

# 2️⃣ Vim Editor

## 📌 What is Vim?

Vim (Vi Improved) is an advanced, modal text editor commonly used by developers, system administrators, and DevOps engineers.

### Install Vim

```bash
sudo apt update
sudo apt install vim
```

### Open a File

```bash
vim example.txt
```

### Create a New File

```bash
vim newfile.txt
```

---

# 🔵 Vim Modes

Vim works using different modes. Press `Esc` to return to Normal Mode from Insert or Visual Mode.

| Mode | Purpose |
|---|---|
| Normal Mode | Navigate and execute editing commands |
| Insert Mode | Type and insert text |
| Visual Mode | Select text |
| Command-Line Mode | Save, quit, search, replace, and execute commands |

> 🔴 **Esc** → Return to Normal Mode.

---

## 🔵 Normal (Command) Mode

| Key | Action |
|---|---|
| `h` | Move left |
| `j` | Move down |
| `k` | Move up |
| `l` | Move right |
| `0` | Move to beginning of line |
| `^` | Move to first non-blank character |
| `$` | Move to end of line |
| `gg` | Go to first line |
| `G` | Go to last line |
| `H` | Move cursor to top of screen |
| `M` | Move cursor to middle of screen |
| `L` | Move cursor to bottom of screen |
| `w` | Move to next word |
| `b` | Move to previous word |
| `u` | Undo |
| `Ctrl + r` | Redo |
| `x` | Delete character |
| `dd` | Delete (cut) current line |
| `D` | Delete from cursor to end of line |
| `dw` | Delete a word |
| `yy` | Yank (copy) current line |
| `yw` | Yank (copy) a word |
| `p` | Paste after cursor |
| `P` | Paste before cursor |
| `cc` | Change (replace) entire line |
| `cw` | Change (replace) a word |

> 💡 **Note:** `cc` and `cw` change text and enter Insert Mode. They are not simply copy or cut commands.

---

## 🟢 Insert Mode

| Key | Action |
|---|---|
| `i` | Insert before cursor |
| `a` | Insert after cursor |
| `I` | Insert at beginning of line |
| `A` | Insert at end of line |
| `o` | Open a new line below |
| `O` | Open a new line above |
| `Esc` | Return to Normal Mode |

### Example

```text
i → Start inserting text
Esc → Return to Normal Mode
```

---

## 🟠 Command-Line Mode

Press `:` in Normal Mode to execute commands.

| Command | Action |
|---|---|
| `:w` | Save file |
| `:q` | Quit |
| `:wq` | Save and quit |
| `:x` | Save and quit |
| `:q!` | Quit without saving |
| `:set number` | Show line numbers |
| `:set nonumber` | Hide line numbers |
| `:set relativenumber` | Show relative line numbers |
| `:set paste` | Enable paste mode |
| `:set nopaste` | Disable paste mode |
| `:help` | Open Vim help |

### Search and Replace

```vim
/old
```

Searches forward for `old`.

```vim
:%s/old/new/g
```

Replaces all occurrences of `old` with `new` throughout the file.

```vim
:%s/old/new/gc
```

Replaces matches with confirmation.

| Symbol | Meaning |
|---|---|
| `%` | Entire file |
| `s` | Substitute |
| `g` | All matches on each line |
| `c` | Confirm each replacement |

### Run a Shell Command

```vim
:!ls
```

Runs `ls` from Vim without exiting the editor.

```vim
:!pwd
```

Displays the current working directory.

---

## 🟣 Visual Mode

Visual Mode is used to select text.

| Key | Action |
|---|---|
| `v` | Character-wise selection |
| `V` | Line-wise selection |
| `Ctrl + v` | Block-wise selection |
| `y` | Copy selected text |
| `d` | Delete selected text |
| `c` | Change selected text |
| `p` | Paste |

### Example

```text
v → Select characters
V → Select complete lines
y → Copy selected content
d → Delete selected content
Esc → Exit Visual Mode
```

---

## 🔍 Vim Navigation and Editing Examples

### Go to a Specific Line

```vim
:10
```

Moves to line 10.

```vim
10G
```

Moves to line 10 in Normal Mode.

### Delete Multiple Lines

```vim
3dd
```

Deletes 3 lines starting from the current line.

### Copy Multiple Lines

```vim
3yy
```

Copies 3 lines.

### Undo and Redo

```text
u       → Undo
Ctrl+r  → Redo
```

---

# 3️⃣ Nano Editor

## 📌 What is Nano?

Nano is a beginner-friendly, terminal-based text editor. It displays important keyboard shortcuts at the bottom of the screen.

### Open or Create a File

```bash
nano example.txt
```

### Common Nano Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl + O` | Write (save) file |
| `Ctrl + X` | Exit Nano |
| `Ctrl + W` | Search text |
| `Ctrl + K` | Cut current line |
| `Ctrl + U` | Paste cut text |
| `Ctrl + G` | Open help |
| `Ctrl + _` | Go to line and column |
| `Alt + U` | Undo |
| `Alt + E` | Redo |

> 💡 **Note:** In Nano, `^` displayed in shortcuts means the `Ctrl` key.

### Practical Example

```bash
nano deploy.sh
```

1. Write your shell script.
2. Press `Ctrl + O` to save.
3. Press `Enter` to confirm the filename.
4. Press `Ctrl + X` to exit.

---

# 4️⃣ Emacs Editor

## 📌 What is Emacs?

Emacs is a highly extensible text editor that can be used through a terminal or graphical interface. It supports programming, scripting, and advanced editing workflows.

### Install Emacs

```bash
sudo apt update
sudo apt install emacs
```

### Open a File

```bash
emacs example.txt
```

### Terminal Mode

```bash
emacs -nw example.txt
```

### Common Emacs Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl + x Ctrl + f` | Open a file |
| `Ctrl + x Ctrl + s` | Save file |
| `Ctrl + x Ctrl + c` | Exit Emacs |
| `Ctrl + s` | Search forward |
| `Ctrl + g` | Cancel current command |
| `Ctrl + k` | Kill text from cursor to end of line |
| `Ctrl + y` | Yank (paste) text |

> 💡 Emacs has a broad ecosystem of extensions and can be customized for development workflows.

---

# 5️⃣ Gedit Editor

## 📌 What is Gedit?

Gedit is a graphical text editor commonly associated with the GNOME desktop environment. It is useful for editing text and code on Linux systems with a graphical interface.

### Install Gedit (Ubuntu)

```bash
sudo apt update
sudo apt install gedit
```

### Open a File

```bash
gedit example.txt
```

### Open a File in the Background

```bash
gedit example.txt &
```

### Features

- Graphical user interface
- Syntax highlighting
- Search and replace
- Multiple document editing (depending on version/configuration)
- Useful for local Linux desktop environments

> ⚠️ **DevOps Tip:** GUI editors may not be available on remote servers. Terminal editors such as Vim and Nano are more suitable for SSH-based administration.

---

# 6️⃣ Sed Stream Editor

## 📌 What is Sed?

`sed` (Stream Editor) is a command-line utility used to process and transform text. Unlike interactive editors, it is commonly used for automated editing in scripts and pipelines.

It is useful for:

- Replacing text in files
- Deleting selected lines
- Printing specific lines
- Automating configuration changes

### 1. Display File Content

```bash
sed -n '1,5p' example.txt
```

Displays lines 1 to 5.

### 2. Replace Text (Output Only)

```bash
sed 's/old/new/g' example.txt
```

Replaces `old` with `new` in the command output. The original file is not changed.

### 3. Replace Text and Save Changes

```bash
sed -i 's/old/new/g' example.txt
```

Modifies the file in place.

> ⚠️ **Warning:** Always verify your target file and replacement pattern before using `sed -i`.

### 4. Delete a Specific Line

```bash
sed '3d' example.txt
```

Removes line 3 from the output.

### 5. Print Specific Lines

```bash
sed -n '2,5p' example.txt
```

Prints lines 2 through 5.

### 6. Create a Backup Before In-Place Editing (GNU sed)

```bash
sed -i.bak 's/old/new/g' example.txt
```

Creates a backup file named `example.txt.bak` before modifying the original.

---

# 🛠️ Practical DevOps Examples

## Example 1: Create and Edit a Shell Script Using Vim

```bash
# Create a script
vim deploy.sh
```

Inside Vim:

```bash
#!/bin/bash

echo "Deployment started"
echo "Application deployed successfully"
```

Save and exit:

```vim
:wq
```

Make the script executable:

```bash
chmod +x deploy.sh
```

Run the script:

```bash
./deploy.sh
```

---

## Example 2: Edit a Configuration File

```bash
vim app.conf
```

Example configuration:

```text
APP_NAME=DevOpsApp
APP_ENV=production
APP_PORT=8080
```

Display the configuration:

```bash
cat app.conf
```

---

## Example 3: Update Configuration Using Sed

Create a configuration file:

```bash
echo "APP_ENV=development" > app.conf
```

Replace the environment:

```bash
sed -i 's/APP_ENV=development/APP_ENV=production/' app.conf
```

Verify the change:

```bash
cat app.conf
```

Expected output:

```text
APP_ENV=production
```

---

## Example 4: Edit a YAML File

Vim and Nano can be used to edit YAML configuration files.

```bash
vim deployment.yaml
```

Example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
```

> ⚠️ YAML is indentation-sensitive. Use consistent spaces and verify the file before deploying.

---

# ⚖️ Vim vs Nano

| Feature | Vim | Nano |
|---|---|---|
| Beginner-friendly | Requires practice | Easy to start |
| Interface | Modal | Direct text editing |
| Advanced navigation | Extensive | Basic |
| Automation workflows | Strong | Suitable for simple edits |
| Available on servers | Common | Common |
| Best for | Advanced editing | Quick configuration changes |

> 💡 **DevOps Recommendation:** Learn Nano for quick edits and Vim for advanced terminal-based editing. Both are useful in Linux environments.

---

# 📚 Editor Command Cheat Sheet

| Command | Description |
|---|---|
| `vi file.txt` | Open file in Vi |
| `vim file.txt` | Open file in Vim |
| `nano file.txt` | Open file in Nano |
| `emacs file.txt` | Open file in Emacs |
| `gedit file.txt` | Open file in Gedit |
| `sed 's/a/b/g' file` | Replace text in output |
| `sed -i 's/a/b/g' file` | Replace text in file |
| `:wq` | Save and quit Vim |
| `:q!` | Quit without saving |
| `Ctrl + O` | Save in Nano |
| `Ctrl + X` | Exit Nano |

---

# 🎯 Learning Outcome

After practicing this chapter, I can:

- Understand different Linux text editors.
- Create and edit files using Vi, Vim, and Nano.
- Navigate Vim modes and perform basic editing operations.
- Use search and replace commands in Vim.
- Edit configuration files in Linux.
- Use Sed for automated text processing.
- Understand the difference between terminal-based and graphical editors.
- Apply text editing skills to DevOps tasks.

---

⭐ **Practice regularly to improve your Linux and DevOps fundamentals.**
