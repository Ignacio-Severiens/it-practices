
# Practice #0005 – Installing VirtualBox and Windows 11 VM on Linux (with KVM Fix)

## 🎯 Goal

Install a Windows 11 virtual machine on Ubuntu 24.04 using VirtualBox from scratch, ensuring the VM is ready for productivity and lab work.

---

## 🔧 Tools and Materials

- Host: Ubuntu 24.04 LTS  
- Windows 11 ISO (from [Microsoft official site](https://www.microsoft.com/software-download/windows11)) → Verify the SHA256 checksum of the Windows 11 ISO before installing with `sha256sum Win11.iso` and compare the output with Microsoft’s official hash.  
- VirtualBox + Extension Pack  
- 8 GB RAM+ (4 GB for host, 4 GB for VM)  
- 50 GB free disk space  

---

## 📘 Commands Used

| Command | Description |
| ------- | ----------- |
| `sha256sum Win11.iso` | Calculates the SHA256 checksum of the downloaded Windows 11 ISO so you can verify its integrity against Microsoft’s official hash. |
| `grep -E  'vmx¦svm' /proc/cpuinfo` | Checks if CPU supports hardware virtualization (Intel VT-x or AMD-V). |
| `sudo apt update && sudo apt upgrade -y` | Updates the package list and upgrades all installed packages to the latest versions. |
| `sudo apt install virtualbox virtualbox-ext-pack -y` | Installs VirtualBox and the extension pack for USB, RDP, and NVMe support from Ubuntu’s official repository. |
| `virtualbox` | Launches VirtualBox GUI. |
| `sudo modprobe -r kvm_intel` | **Temporarily** unloads the Intel KVM kernel module (required because VirtualBox and KVM can conflict). The change is lost after reboot. |
| `echo -e "blacklist kvm\nblacklist kvm_intel\nblacklist kvm_amd" ¦ sudo tee /etc/modprobe.d/blacklist-kvm.conf` | **Permanently** blacklists KVM-related modules so they won’t load at boot. |
| `sudo apt install lm-sensors` | Installs the `lm-sensors` package to monitor CPU and system temperatures. |
| `sudo sensors-detect` | Detects available hardware sensors. Choose “yes” for all prompts. |
| `sensors` | Displays system temperature, fan speed, and voltage readings. |
| `sensors ¦ grep 'Core’` | `lm-sensors` reads system sensor data; `grep 'Core'` filters for CPU core temperatures. |

> _Note: The pipe character (`|`) is a column separator in Markdown tables and breaks formatting inside table cells. To work around this, the pipe in commands inside tables is replaced by the similar-looking “broken bar” (`¦`). If you copy commands from this table, replace `¦` with `|` before running._

---

## 📝 Steps Performed

### Step 0 – Check CPU virtualization support

```bash
grep -E 'vmx¦svm' /proc/cpuinfo
```

- **vmx** → Intel VT-x  
- **svm** → AMD-V  
- If neither appears, your CPU cannot run hardware-accelerated VMs.

### Step 1 – Install VirtualBox

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install virtualbox virtualbox-ext-pack -y
virtualbox
```

### Step 2 – Create & Configure VM

1. **New VM**  
   - Name: Win11  
   - Type: Microsoft Windows → Windows 11 (64-bit)  
   - RAM: 4 GB (minimum)  
   - CPUs: 2+ cores  
   - Enable EFI  

2. **Virtual Disk**  
   - 50 GB, VDI, dynamically allocated  

3. **Mount ISO**  
   - Settings → Storage → Add Optical Drive → Select `win11.iso`  

4. **Performance tweaks**  
   - System: Enable I/O APIC  
   - Display: 128 MB VRAM, enable 3D acceleration  

### Step 3 – Fix VT-x/KVM Conflict (if needed)

Temporarily:

```bash
sudo modprobe -r kvm_intel   # or kvm_amd for AMD CPU
echo -e "blacklist kvm\nblacklist kvm_intel\nblacklist kvm_amd" | sudo tee /etc/modprobe.d/blacklist-kvm.conf
```

Permanently:
```bash
sudo modprobe -r kvm_intel   # or kvm_amd for AMD CPU
echo -e "blacklist kvm\nblacklist kvm_intel\nblacklist kvm_amd" | sudo tee /etc/modprobe.d/blacklist-kvm.conf
```

### Step 4 – Install Windows 11

Start VM and follow the wizard until reaching the desktop.

### Step 5 – Post-install Optimization

- **Enable full screen & better integration**  
  - Devices → Insert Guest Additions CD image → Run installer → Restart  

- **Check CPU temps**  
```bash
sudo apt install lm-sensors -y
sudo sensors-detect   # choose yes to all prompts
sensors | grep 'Core'
```

---

## 💡 Troubleshooting

| Problem | Cause | Solution |
| ------- | ----- | -------- |
| VM won’t start | KVM blocking VT-x | `sudo modprobe -r kvm_intel` (or `kvm_amd` for AMD CPUs) |
| Guest Additions CD not found | ISO not present on host | Download Guest Additions ISO from VirtualBox website, mount in VM → Run `VBoxWindowsAdditions.exe` |
| Fullscreen works but screen is small / scaling issues | Guest Additions not installed or display scaling not applied | Install Guest Additions → Restart VM → Use `Host+F` → Adjust Windows Display scaling (Settings → System → Display → Scale & layout) |
| Shared clipboard / drag & drop not working | Guest Additions missing or not installed correctly | Install Guest Additions → Restart VM → Ensure VM settings (Devices → Shared Clipboard / Drag & Drop) are Bidirectional |

---

## 📚 Learned

- VirtualBox setup on Linux  
- VM creation & OS installation  
- Resolving hypervisor conflicts  

---

## ✅ Final Status

Windows 11 VM runs smoothly, full screen, shared clipboard and drag-and-drop enabled.  

## 📘 Extended Version

A more detailed version of this documentation (including screenshots and Spanish translation) is available on Notion:

📎 [Notion – Practice #0005 – Installing VirtualBox and Windows 11 VM on Linux (with KVM Fix)](https://www.notion.so/Practice-0005-2025-08-15-Installing-VirtualBox-and-Windows-11-VM-on-Linux-with-KVM-Fix-24deb94034d9805ea999d46e3dd4f0ad?source=copy_link)
