# Practice #0007 – Exploring Windows Command-Line Tools

### 🎯 Goal

Understand and practice the use of key Windows command-line tools for system management, maintenance, and troubleshooting.

Learn how to differentiate between standard and elevated (administrator) privileges, execute diagnostic and maintenance commands, and interpret their outputs.

---

### 🔧 Tools and Materials
- Windows 11 VM (from [Practice #0005](https://www.notion.so/Practice-0005-2025-08-15-Installing-VirtualBox-and-Windows-11-VM-on-Linux-with-KVM-Fix-24deb94034d9805ea999d46e3dd4f0ad))
- Command Prompt (`cmd.exe`)
- Administrator access (Run as Administrator)
- A test directory and a few text files for experimentation

---

## 🧭 Tools and Commands Explored

### Access and Privileges
- **Standard users** can run most commands but cannot perform system-level tasks.
- **Administrator (elevated) privileges** are required for commands that affect system settings or files.

**How to open an elevated Command Prompt:**
- Right-click Command Prompt → *Run as Administrator*
- Or type `cmd` in Start, then press **Ctrl + Shift + Enter**

> 💡 **Pro Tip:** Use `Ctrl + Shift + Right-Click` in File Explorer → *Open Command Window Here* to start CMD directly in that folder.

📸 *Screenshot:* `01-administrator-command-prompt.png`

---

### File System Navigation and Management
| Command | Description |
|----------|--------------|
| `dir` | Lists files and folders in the current directory. |
| `cd` | Changes the current directory. |
| `mkdir` | Creates a new folder (directory). |
| `rmdir` | Removes an empty folder. |
| `copy` | Copies files from one location to another. |
| `del` | Deletes one or more files. |

**Notes:**
- `del` removes files, `rmdir` removes folders (only if empty).
- GUI delete removes folders and contents recursively.
- **Paths:** Absolute (`C:\Users\Name`) vs Relative (`..\Documents`).

📸 *Screenshot:* `02-file-system-navigation.png`

---

### Disk and File System Tools
- **`chkdsk`** – Checks and repairs disk errors (file system corruption, bad sectors, lost clusters).
- **`format`** – Erases all data on a partition and creates a new file system (e.g., NTFS, FAT32). ⚠️ *Permanently deletes all data.*
- **`diskpart`** – Advanced disk partitioning tool (create, delete, clean disks/partitions). ⚠️ *Risky — a single wrong command can wipe entire drives.*

📸 *Screenshot:* `03-diskpart.png`

---

### System Information & Diagnostics
| Command | Description |
|----------|--------------|
| `hostname` | Displays the computer’s network name. |
| `whoami` | Shows the currently logged-in user account. |
| `winver` | Opens a window showing Windows edition and build. |
| `systeminfo` | Displays detailed system configuration info. |

📸 *Screenshots:*
- `04-hostname-and-whoami.png`
- `05-winver.png`
- `06-systeminfo.png`

---

### Group Policy and System Integrity
| Command | Description |
|----------|--------------|
| `gpupdate /force` | Refreshes Group Policy settings immediately. |
| `gpresult /r` | Displays applied Group Policy settings. |
| `sfc /scannow` | Scans and repairs corrupted or missing system files. |

**Notes:**
- **Group Policy** manages system/user settings.
- **SFC** repairs local Windows files.  
Together they maintain system stability and integrity.

📸 *Screenshots:*
- `07-gpupdate-and-gpresult.png`
- `08-sfc.png`

---

### Safe Command Testing and Learning
- Use `help` and `<command> /?` for built-in documentation.
- Always test safely and read help before running commands that modify files or configurations.

📸 *Screenshot:* `09-help-dir.png`

---

### 📚 Learned
- Difference between standard and administrative privileges.
- Navigation, management, and troubleshooting using Windows CLI.
- Key system utilities (`chkdsk`, `sfc`, `gpupdate`, etc.).
- Safe self-learning via built-in command help.

---

## 📘 Extended Version

A more detailed version of this documentation (including screenshots and Spanish translation) is available on Notion:

📎 [Notion – Practice #0007 – Exploring Windows Command-Line Tools](https://www.notion.so/Practice-0007-2025-10-23-Exploring-Windows-Command-Line-Tools-294eb94034d980c58bfcdafdda691727?source=copy_link)

### 🇪🇸 Español

Si lo deseas, puedes leer una versión en español de esta práctica en [Notion](https://www.notion.so/Practice-0007-2025-10-23-Exploring-Windows-Command-Line-Tools-294eb94034d980c58bfcdafdda691727?pvs=97#295eb94034d980d89c25d935b554798d).

