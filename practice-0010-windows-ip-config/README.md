# Practice #0010 – Exploring Windows IP Address Configuration

### 🎯 Goal

Explore how Windows obtains and manages IP configurations through **DHCP**, **APIPA**, **Static**, and **Alternate Configuration** modes, and learn to verify and troubleshoot them using GUI and CLI tools.

---

### 🖥️ Testing Environment

**Windows 11 Pro 23H2 (Build 26100)** running in **VirtualBox 7.0.16 on Ubuntu**.

---

### 🧰 Tools and Materials

- Windows 11 VM (from [Practice #0005](https://github.com/Ignacio-Severiens/it-practices/tree/main/practice-0005-virtualbox-win11))
- Command Prompt (`cmd.exe`)
- Network connectivity (router or VirtualBox NAT)
- Administrator privileges (for some commands)

---

## ⚙️ Section 1 – Examining IP Configuration (DHCP Active)

**Steps:**

1. Run `ipconfig` and `ipconfig /all`.
2. Identify your network adapter’s configuration:
    - **IPv4 address:** `10.0.2.15`
    - **Subnet mask:** `255.255.255.0` (`/24`)
    - **Default Gateway:** `10.0.2.2`
    - **DHCP Server:** `10.0.2.2`
    - **DNS Server (IPv4):** `10.0.2.3`
3. Confirm that **DHCP Enabled** is set to **Yes**.

📸 *Screenshot:* `01-ipconfig-dhcp.png`  
*`ipconfig /all` output showing DHCP is enabled*

**Notes:**

- The DHCP lease duration is 1 day.
- The IP address is obtained dynamically from the DHCP server (`10.0.2.2`).
- After running `ipconfig /release` followed by `ipconfig /renew`, the same IPv4 address (`10.0.2.15`) was re-assigned — likely because the DHCP server’s lease table still reserved that address for my MAC address.

**Interpretation:**

This confirms the adapter is configured via DHCP and successfully communicating with the DHCP and DNS servers.

---

## 🧩 Section 2 – Observing APIPA Behavior after DHCP Failure

**Objective:**

Simulate a DHCP failure in Windows and observe the Automatic Private IP Addressing (APIPA) behavior (`169.254.x.x` addresses).

**Steps:**

1. **Prepare the VM for DHCP failure simulation**
    - To simulate DHCP failure, I created a **NAT Network** in VirtualBox with **DHCP disabled** and attached my VM to this network.
    
    📸 *Screenshot:* `02-nat-network-dhcp-disabled.png`  
    *NAT network created with DHCP disabled to simulate DHCP failure.*
    
2. **Power cycle the VM**
    - Turn off and then turn on the VM (do not just restart) to apply the new DHCP configuration.
3. **Check IP configuration**
    - Open a Command Prompt inside the VM and run:
        ```
        ipconfig /release
        ipconfig /renew
        ipconfig
        ```
    - Verify that the IPv4 address is in the **`169.254.x.x` range** (APIPA).

    📸 *Screenshot:* `03-apipa-address.png`  
    *`ipconfig` output showing APIPA address (`169.254.203.97`)*

**Observations / Notes:**

- In APIPA mode:
    - No Default Gateway is assigned.
    - No DHCP server information is available.
    - IPv4 DNS servers are not functional (IPv6 entries may still appear).
- APIPA addresses only allow local network communication; hosts outside the VM network cannot be reached (e.g., pinging the internet fails).

**Troubleshooting Tips:**

- Simply disconnecting the VM’s network adapter results in “Media disconnected”; no APIPA address is assigned.
- Switching the VM to a NAT Network with DHCP disabled is required to simulate DHCP failure.
- Restarting Windows is not enough to refresh DHCP/APIPA settings — the VM must be fully powered off and then on after changing network settings. → Later I’ve noticed I could have switched to another network setting and back to the NAT Network for this to work…

---

## 🧍 Section 3 – Setting a Static IP

**Static IPs are necessary when:**

- A device needs a **fixed address** (e.g., servers, printers, network shares).
- **Port forwarding** or remote access requires a consistent IP.
- **Network services** rely on predictable addressing (DNS, DHCP reservations, VPNs).
- **IoT devices** or equipment that must always be reachable without relying on DHCP.

In short, use a static IP whenever the address must **not change**.

**Steps:**

1. Control Panel → Network and Sharing Center → Change adapter settings.
2. Right-click your adapter → Properties → IPv4 → Properties.
3. Manually set:
    - IP: `10.0.2.50`
    - Subnet mask: `255.255.255.0`
    - Default gateway: `10.0.2.1` (your router IP)
    - DNS: `9.9.9.9` (Quad9)

📸 *Screenshot:* `04-static-ip-config.png`  
*Static IP configuration in the Control Panel*

📸 *Screenshot:* `05-static-ip-cmd.png`  
*Static IP verified in the command line output when running `ipconfig` and `ipconfig /all`*

**Notes:**

- I could not use `ipconfig /renew` nor `ipconfig /release` with a static IP configured. I got the message:

    ```
    The operation failed as no adapter is in the state permissible for this operation.
    ```

    📸 *Screenshot:* `06-failed-ipconfig-release.png`  
    *`ipconfig /release` failed with a static IP configured*

---

## 🔄 Section 4 – Alternate Configuration

**Steps:**

1. In the same **IPv4 Properties** window, open the **Alternate Configuration** tab.
2. Select **User Configured** and define a backup IP, subnet mask, gateway, and DNS.
3. Disable or disconnect DHCP again and observe how Windows switches automatically.

📸 *Screenshot:* `07-alternate-ip-config.png`  
*Alternate IP configuration in the Control Panel*

**Notes:**

- The Alternate Configuration activates only after Windows fails to obtain an IP from a DHCP server.
- ⚠️ **Important:** In my test, the adapter initially showed the manual IP but no Default Gateway. Only after restarting the VM did Windows fully apply the Alternate Configuration, including gateway and DNS. This is a common “gotcha” — a restart or network reinitialization may be required to trigger it.
- Once a DHCP server becomes available again, Windows automatically switches back to using the DHCP-assigned IP configuration. No reboot or manual change is needed.
- Alternate IPs ensure network continuity for mobile or laptop users who move between networks — for example, when a device can’t reach the corporate DHCP server but still needs a working local configuration (e.g., static testing, admin access, or local file sharing).

---

## 🧠 Section 5 – Testing and Validation

**Steps:**

1. Use `ping` to verify connectivity under each configuration (DHCP, APIPA, static, alternate).
2. Test the loopback address (`ping 127.0.0.1`) to confirm TCP/IP stack integrity.

**Notes:**

- The loopback address (`127.0.0.1`) tests only the local TCP/IP stack — it never leaves your computer. This verifies that your network interface, drivers, and TCP/IP services are functioning correctly, regardless of physical or virtual network connectivity.

**Can you reach other machines in each configuration type?**

| **Configuration Type** | **Reachability** | **Notes** |
| --- | --- | --- |
| **DHCP** | ✅ Full network & internet access | Works normally when a gateway and DNS are provided by the DHCP server. |
| **APIPA** | ⚠️ Local only | Can communicate with devices in the same `169.254.x.x` range, but no internet or routing beyond the local link. |
| **Static** | ✅ / ⚠️ Depends | Full connectivity if the IP, gateway, and DNS are configured correctly. Misconfiguration breaks access. |
| **Alternate** | ✅ Local or routed | Functions like a static setup; provides continuity without DHCP if gateway and DNS are correctly defined. |

---

## ⚡ Quick Command Reference

| **Command** | **Purpose / Notes** |
| --- | --- |
| `ipconfig` | Show current IP configuration for all adapters. |
| `ipconfig /all` | Show detailed information, including DHCP, DNS, MAC, and lease info. |
| `ipconfig /release` | Release the current DHCP lease (only works if DHCP is enabled). |
| `ipconfig /renew` | Request a new DHCP lease (only works if DHCP is enabled). |
| `ping <IP or hostname>` | Test connectivity to another host or the loopback (`127.0.0.1`). |

---

## 📚 Learned

- Difference between DHCP, APIPA, Static, and Alternate configurations.
- How to manually manage and verify IP settings.
- The role of DHCP lease, DNS, and gateway in connectivity.
- How to diagnose connection loss via `ipconfig` and `ping`.
- How Windows automatically falls back when DHCP fails.
- How to simulate and recover from DHCP failure conditions safely in a virtualized environment.

---

## ✅ Final Status

- All configurations tested (DHCP, APIPA, Static, Alternate).
- Screenshots captured.
- VM reverted to original DHCP-enabled NAT configuration after testing.

---

## 📘 Extended Version

A more detailed version of this documentation (including screenshots and Spanish translation) is available on Notion:

📎 [Notion – Practice #0009 – Managing and Testing Windows Defender Firewall Rules](https://www.notion.so/Practice-0010-2025-11-01-Exploring-Windows-IP-Address-Configuration-29deb94034d98086873eee8609b586a4?source=copy_link)

### 🇪🇸 Español

Si lo deseas, puedes leer una versión en español de esta práctica en [Notion](https://www.notion.so/Practice-0010-2025-11-01-Exploring-Windows-IP-Address-Configuration-29deb94034d98086873eee8609b586a4?pvs=97#29deb94034d9815c8db8f2005e407652).
