# Practice #0001 – Safe Analysis of Suspicious USB Drive

## 🎯 Goal

To safely analyze a potentially infected USB drive on a Linux system without opening or executing files, using ClamAV antivirus for static analysis.

---

## 🔧 Tools & Materials

- OS: Ubuntu 24.04 LTS
- Terminal
- USB flash drive with unknown content
- Antivirus: ClamAV
- Commands: `lsblk`, `df -h`, `clamscan`, `systemctl`, `freshclam`

---

## 📝 Steps

1. Updated system:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Installed ClamAV:
   ```bash
   sudo apt install clamav -y
   ```
3. Tried to update virus DB:
   ```bash
   sudo freshclam
   ```
   ❌ Failed due to locked log file.
4. Fixed by stopping background service:
   ```bash
   sudo systemctl stop clamav-freshclam
   sudo freshclam
   sudo systemctl start clamav-freshclam
   ```
5. Plugged in USB (did **not** open file explorer).
6. Identified mount point:
   ```bash
   lsblk
   df -h
   ```
   → Found at `/media/ignacio/4889-7BEA`
7. Ran scan:
   ```bash
   clamscan -r /media/ignacio/4889-7BEA
   ```
   ✅ No infected files found.

---

## 💡 Issues & Solutions

| Issue      | Fix |
|------------|-----|
| `freshclam` failed due to locked log file (`/var/log/clamav/freshclam.log`) | Stopped `clamav-freshclam` daemon manually before updating |

---

## 📚 What I Learned

- How to handle suspicious USB drives without risk
- Installing and managing ClamAV
- Using `lsblk` and `df` to identify mounted devices
- Performing antivirus scans from CLI
- Avoiding automatic execution of unknown files

---

## 🔄 Next Steps

- Try scanning in a VM (Kali or Windows) for better isolation
- Learn to use VirusTotal API for file hash checks
- Automate scan process with a Bash script

---

## ✅ Final Status

The USB drive was scanned successfully and no malware was detected.

---

## 📷 Screenshots

See folder: [`/screenshots`](./screenshots)

---

## 🇪🇸 Español

> Este README está escrito en inglés como idioma principal. Si lo deseás, podés leer una versión en español de esta práctica en [Notion](https://www.notion.so/Practice-0001-2025-07-06-Safe-Analysis-of-Suspicious-USB-Drive-228eb94034d980f9933ac5de9042d4f0?source=copy_link).
