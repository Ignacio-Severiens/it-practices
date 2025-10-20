# Practice #0006 – Exploring Windows System Tools (Monitoring, Maintenance & Registry)

### 🎯 Goal

Get familiar with essential Windows system utilities used for monitoring, maintenance, and troubleshooting.

---

### 🔧 Tools and Materials
- Windows 11 VM (from [Practice #0005](https://www.notion.so/Practice-0005-2025-08-15-Installing-VirtualBox-and-Windows-11-VM-on-Linux-with-KVM-Fix-24deb94034d9805ea999d46e3dd4f0ad?source=copy_link))
- Administrator account access
- Built-in Windows utilities:
  - Task Manager (`taskmgr.exe`)
  - Resource Monitor (`resmon.exe`)
  - System Information (`msinfo32.exe`)
  - Microsoft Management Console (`mmc.exe`)
  - System Configuration (`msconfig.exe`)
  - Disk Cleanup (`cleanmgr.exe`)
  - Defragment and Optimize Drives (`dfrgui.exe`)
  - Registry Editor (`regedit.exe`)

---

## 🧭 Tools Explored

This practice focuses on the most important built-in utilities every technician should know for monitoring performance, managing configurations, and maintaining a healthy Windows system.

### Task Manager
- Provides real-time system statistics.
- Tabs:
  - **Processes** – Overview of running apps and background processes.
  - **Performance** – Graphs for CPU, memory, disk, network, and GPU usage.
  - **App History** – Tracks resource usage for Store apps.
  - **Startup** – Manages startup applications.
  - **Users** – Shows resource usage per logged-in user.
  - **Details** – Advanced process view (PID, priority, etc.).
  - **Services** – Manage running services.

📸 *Screenshot:* `01-task-manager-cpu-performance.png`

---

### Resource Monitor (`resmon.exe`)
- Offers detailed real-time data by resource type (CPU, Disk, Network, Memory).
- Allows process-level analysis beyond Task Manager.

📸 *Screenshot:* `02-resmon-cpu-tab.png`

---

### System Information (`msinfo32.exe`)
- Displays full hardware and software configuration.
- Useful for:
  - Checking CPU, RAM, BIOS, and drivers.
  - Viewing IRQs and system conflicts.
  - Detecting hardware or driver failures.

📸 *Screenshot:* `03-msinfo32-system-summary.png`

---

### Microsoft Management Console (`mmc.exe`)
- Framework to manage administrative “snap-ins” (e.g., Services, Device Manager).
- Customizable tool for system administration.

📸 *Screenshot:* `04-mmc.png`

---

### System Configuration (`msconfig.exe`)
- Used to troubleshoot startup and configure boot options.
- Tabs:
  - **General** – Startup modes.
  - **Boot** – Safe boot, timeout, etc.
  - **Services** – Manage service startup.
  - **Startup** – (Redirected to Task Manager in modern Windows).
  - **Tools** – Quick access to diagnostics utilities.

📸 *Screenshot:* `05-msconfig-boot-tab.png`

---

### Disk Cleanup (`cleanmgr.exe`)
- Frees disk space by removing temporary and system files.
- Safe deletions:
  - Temporary files, Recycle Bin, Windows Update cleanup, `Windows.old`, etc.
- Review before confirming — avoid removing driver packages unless necessary.

📸 *Screenshot:* `06-cleanmgr.png`

---

### Defragment and Optimize Drives (`dfrgui.exe`)
- Optimizes storage by rearranging fragmented files.
- **Note:** SSDs don’t benefit from traditional defragmentation; Windows uses TRIM instead.

📸 *Screenshots:*
- `07-dfrgui-before.png`
- `08-dfrgui-after.png`

---

### Registry Editor (`regedit.exe`)
- Central database for system and app configuration.
- Affects kernel, drivers, services, and UI.
- ⚠️ **Always back up** before editing: *File → Export*.

📸 *Screenshot:* `09-regedit.png`

---

### 📚 Learned
- Monitoring and troubleshooting using native Windows tools.
- Difference between monitoring (Task Manager) and configuration (msconfig, MMC).
- Safe navigation of the Windows Registry.
- Understanding built-in maintenance utilities for troubleshooting.

---

## 📘 Extended Version

A more detailed version of this documentation (including screenshots and Spanish translation) is available on Notion:

📎 [Notion – Practice #0006 – Exploring Windows System Tools (Monitoring, Maintenance & Registry)](https://www.notion.so/Practice-0006-2025-10-20-Exploring-Windows-System-Tools-Monitoring-Maintenance-Registry-292eb94034d980e69e09fc37b48424c9?source=copy_link)

### 🇪🇸 Español

Si lo deseas, puedes leer una versión en español de esta práctica en [Notion](https://www.notion.so/Practice-0006-2025-10-20-Exploring-Windows-System-Tools-Monitoring-Maintenance-Registry-292eb94034d980e69e09fc37b48424c9?pvs=97#292eb94034d98049a5e4f7366ec46cfa).

