# Practice #0002 – Secure Wiping and Formatting of USB Stick

## 🎯 Goal

Securely erase the entire content of a USB drive using Ubuntu terminal tools, preventing any previous files from being recovered, and prepare it for reuse.

## 🔧 Tools & Materials

- Ubuntu 24.04 LTS
- Terminal
- 4GB USB flash drive (previously scanned)
- Commands: `lsblk`, `df -h`, `umount`, `dd`, `mkfs.vfat`, `udisksctl`, `fdisk`, `kill`
- Basic terminal knowledge

## 📘 Commands Used

| Command | Description |
| --- | --- |
| `lsblk` | Lists all block devices (disks, partitions, USBs, etc.). Helps identify the correct device (e.g., `/dev/sdb`). |
| `df -h` | Shows disk space usage of mounted file systems. Useful to check if the USB is mounted. |
| `sudo umount /dev/sdXN` | Unmounts a mounted partition. Required before writing or formatting. |
| `sudo dd if=/dev/zero of=/dev/sdX bs=4M status=progress` | Overwrites the entire device with zeros. Destroys all data. Very dangerous if run on the wrong disk. |
| `sudo mkfs.vfat /dev/sdX -n LABEL` | Formats the device with a FAT32 filesystem and assigns an optional label. |
| `sudo fdisk /dev/sdX` | Tool to manually create/delete partitions in terminal. |
| `udisksctl power-off -b /dev/sdX` | Safely powers off and unmounts external USB devices. |
| `sudo kill -9 PID` | Forcefully kills a stuck process. Used when `udisksctl` froze. |

⚠️ Use extreme caution with `dd` and `mkfs`. Always verify the correct device using `lsblk`.

## 📝 Steps Performed

1. **Connected the USB drive**
   - Inserted the USB without opening the file manager.
   - Verified with `lsblk`: `/dev/sda` and `/dev/sda1` detected.
2. **Unmounted the partition**
   - Ran: `sudo umount /dev/sda1`
3. **Secure wipe using `dd`**
   - Executed: `sudo dd if=/dev/zero of=/dev/sda bs=4M status=progress`
   - Waited for the process to complete until `No space left on device`.
4. **Problem: Formatting got stuck**
   - Tried: `sudo mkfs.vfat /dev/sda -n USBLIMPIO`
   - Terminal froze. Opened another terminal and ran: `lsblk -f`
   - Confirmed no partition or filesystem had been created.
5. **Created partition manually**
   - Used `fdisk` to create a new partition table and partition: `sudo fdisk /dev/sda`
   - Inside `fdisk`:
     - `o` (new DOS partition table)
     - `n` (new partition)
     - `p`, `1`, default values
     - `t`, `c` (W95 FAT32 LBA)
     - `w` (write and exit)
6. **Formatted partition**
   - Ran: `sudo mkfs.vfat /dev/sda1 -n USBLIMPIO`
7. **Safe removal**
   - Tried: `udisksctl power-off -b /dev/sda`
   - It froze again. Opened another terminal, found and killed the process:

```bash
ps aux | grep udisksctl
sudo kill -9 PID
```

   - Safely unplugged the USB drive afterward.

## 💡 Problems Encountered (and Solutions)

| Problem | Solution |
| --- | --- |
| `mkfs.vfat` froze when formatting `/dev/sda` directly | Created a partition with `fdisk`, then formatted `/dev/sda1` |
| `udisksctl power-off` froze | Killed the process with `kill -9` from another terminal |
| No partition found after `dd` | This is normal; `dd` wipes everything including partition tables |

## 📚 What I Learned

- How to securely wipe a USB flash drive using `dd`.
- Why it's important to format the partition (e.g., `/dev/sda1`) instead of the whole disk.
- How to use `fdisk` to manually rebuild a partition table.
- How to recover from stuck processes and deal with frozen terminal commands.


## ✅ Final Status

The USB flash drive was securely wiped, partitioned, and reformatted using FAT32. It’s now ready and safe for reuse.

## 📷 Screenshots

See folder: `/screenshots`

## 🇪🇸 Español

Una versión completa de esta práctica también está disponible en español en [Notion](https://www.notion.so/Practice-0002-2025-07-06-Secure-Wiping-and-Formatting-of-USB-Stick-228eb94034d980c89e93f435dfc77332?source=copy_link).
