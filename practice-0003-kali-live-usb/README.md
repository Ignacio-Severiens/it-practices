
# Practice #0003 – Creating Kali Live USB

## 🎯 Goal

Create a Kali Linux Live USB to boot Kali from a USB stick.

---

## 🔧 Tools and Materials

- Ubuntu 24.04 LTS (host OS)
- USB drive (at least 16 GB, preferably 32 GB and USB 3.0)
- ISO image: `kali-linux-2025.2-live-amd64.iso`
- Terminal
- Preinstalled tools: `lsblk`, `dd`, `mount`, `umount`

---

## 📘 Commands Used

| Command | Description |
| ------- | ----------- |
| `lsblk` | Lists connected storage devices to identify the USB stick |
| `sudo dd if=... of=...` | Writes the Kali ISO to the USB drive (⚠️ destructive) |

⚠️ **`dd` is dangerous. Selecting the wrong device (e.g. `/dev/sda`) could wipe your entire hard drive.**

---

## 📝 Steps Performed

1. **Downloaded the ISO** from the official Kali website (under the **Live Boot** section).  
   ✅ At the time of writing: `kali-linux-2025.2-live-amd64.iso`

2. **Plugged in the USB drive**

3. **Identified the USB device** using `lsblk`.

4. **Unmounted the device** with:  
   ```bash
   sudo umount /dev/sda1
   ```  
   Then rechecked with `lsblk` to confirm it was no longer mounted.  
   ⚠️ Writing with `dd` while the device is mounted can cause errors or corruption.

5. **Flashed the ISO** using `dd`:
   ```bash
   sudo dd if=~/Downloads/kali-linux-2025.2-live-amd64.iso of=/dev/sda bs=4M status=progress oflag=sync
   ```
   - `if=...` → Input file (Kali ISO)
   - `of=...` → Output device (`/dev/sda`, not `/dev/sda1`)
   - `bs=4M` → Block size (faster writing)
   - `status=progress` → Shows progress in real time
   - `oflag=sync` → Flushes write buffers for each block (safer)

6. **Powered off the USB** device to force a rescan:  
   ```bash
   udisksctl power-off -b /dev/sda
   ```  
   Then unplugged and reinserted the USB stick. This ensures the OS detects the new partition structure correctly.

7. **Rebooted into BIOS/UEFI** and **disabled Secure Boot**.

8. ⚠️ **Important:** A standard reboot wasn’t enough — I had to **shut down and cold boot** the system for BIOS changes to take effect and for the USB to appear as bootable.

9. Booted into the **Kali Linux Live System** successfully from the USB.

---

## 💡 Problems Encountered (and Solutions)

| Problem | Solution |
| ------- | -------- |
| `dd` permission error | Used `sudo` |
| “Out of policy” / Secure Boot violation | Disabled Secure Boot in BIOS |
| USB not listed as boot option | Performed a full shutdown instead of reboot to apply BIOS changes |

---

## 📚 What I Learned

- How to create a clean, bootable Kali Linux Live USB using terminal tools.
- Why `dd` must be used cautiously and only on the correct device.
- That Secure Boot can silently block booting from Linux USBs.
- That a **cold boot is sometimes required** for BIOS changes to register.

---

## ✅ Final Status

✅ Kali Linux Live USB successfully created and booted.

---

## ⏭️ Next Steps

→ Add persistence to the USB for saving files and configurations across sessions.

---

## 📘 Extended Version

A full and detailed version of this documentation (including screenshots and a Spanish translation) is available on Notion:

📎 [Notion – Practice #0003 – Creating Kali Live USB](https://www.notion.so/Practice-0003-2025-07-XX-Creating-Kali-Live-USB-229eb94034d980e18079ed8d666e4bca)
