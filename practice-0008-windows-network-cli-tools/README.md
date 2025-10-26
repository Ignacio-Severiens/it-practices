# Practice #0008 – Exploring Windows Network Command-Line Tools

### 🎯 Goal

Understand and practice the use of key Windows **network command-line tools** used for connectivity testing, name resolution, and network diagnostics.

Learn to interpret outputs, identify common network issues, and combine multiple tools for comprehensive troubleshooting.

---

### 🔧 Tools and Materials
- Windows 11 VM (from [Practice #0005](https://github.com/Ignacio-Severiens/it-practices/tree/main/practice-0005-virtualbox-win11))
- Command Prompt (`cmd.exe`) – *Run as Administrator*
- Internet connectivity

---

## 🧭 Tools and Commands Explored

### 🖧 Network Configuration – `ipconfig`

| Command | Description |
|----------|--------------|
| `ipconfig` | Displays basic network adapter info such as IP address, subnet mask, and default gateway. |
| `ipconfig /all` | Shows detailed configuration for all adapters (includes MAC, DNS, and DHCP details). |
| `ipconfig /release` | Releases the current DHCP-assigned IP address. |
| `ipconfig /renew` | Requests a new IP from the DHCP server after release or connection loss. |
| `ipconfig /flushdns` | Clears the local DNS cache to force fresh lookups and resolve outdated entries. |

📸 *Screenshot:* `01-ipconfig-vs-ipconfig-all.png`

---

### 🏓 Connectivity Testing – `ping`

| Command | Description |
|----------|--------------|
| `ping 127.0.0.1` | Tests the local network stack (loopback). |
| `ping <default gateway>` | Verifies LAN connectivity. |
| `ping 8.8.8.8` | Tests external Internet reachability (Google DNS). |
| `ping google.com` | Tests DNS resolution + connectivity. |

**Notes:**
- **Request timed out:** No reply received (firewall, latency, or congestion).  
- **Destination host unreachable:** No route to target host (routing problem).  
- Some hosts block ICMP replies for security or performance reasons.

📸 *Screenshot:* `02-ping.png`

---

### 📊 Active Connections Analysis – `netstat`

| Command | Description |
|----------|--------------|
| `netstat` | Displays active TCP connections. |
| `netstat -a` | Shows all connections and listening ports. |
| `netstat -b` | Displays the executable that opened each connection (Admin only). |
| `netstat -n` | Shows numeric addresses and ports (no name resolution). |
| `netstat -anob` | Full diagnostic view – all connections, ports, and owning programs. |

**Notes:**
- `LISTENING` = waiting for incoming connections.  
- `ESTABLISHED` = active connection.  
- `TIME_WAIT` = connection recently closed.  
- Useful for identifying suspicious connections or unknown executables.  

📸 *Screenshot:* `03-netstat.png`

---

### 🌍 DNS Diagnostics – `nslookup`

| Command | Description |
|----------|--------------|
| `nslookup google.com` | Forward lookup – domain → IP. |
| `nslookup 8.8.8.8` | Reverse lookup – IP → domain (if available). |

**Notes:**
- Displays which DNS server is responding.  
- Helps determine if connectivity issues are DNS-related or network-related.

📸 *Screenshot:* `04-nslookup.png`

---

### 🧱 Network Management – `net`

| Command | Description |
|----------|--------------|
| `net view` | Lists shared resources on the local network. |
| `net use` | Connects/disconnects network drives or printers. |
| `net user` | Displays or manages local user accounts. |
| `net accounts` | Shows or modifies account and password policies. |

**Notes:**
- `net view` requires a reachable network.  
- `net use` may fail in workgroups without shared credentials.  
- Useful for managing users and shares in both local and domain environments.

📸 *Screenshot:* `05-net-user.png`

---

### 🛰️ Route Tracing and Analysis – `tracert` & `pathping`

| Command | Description |
|----------|--------------|
| `tracert 8.8.8.8` | Traces packet route to the target IP. |
| `tracert google.com` | Traces route and tests DNS resolution. |
| `pathping google.com` | Combines `tracert` and `ping` to show packet loss per hop. |

**Notes:**
- `*` in results = router didn’t reply to ICMP but is forwarding traffic.  
- `pathping` helps identify packet loss or bottlenecks across hops.  
- Some routers intentionally block or deprioritize ICMP traffic.

📸 *Screenshots:*  
- `06-tracert.png`  
- `07-pathping.png`

---

### 📚 Learned

- How to check and renew IP configurations (`ipconfig`).
- How to test and interpret network connectivity (`ping`).
- How to identify open connections and processes (`netstat`).
- How to troubleshoot DNS issues (`nslookup`).
- How to trace routes and packet loss (`tracert`, `pathping`).
- How to manage users and network resources (`net`).
- Importance of combining multiple CLI tools for complete diagnostics.

---

## 📘 Extended Version

A more detailed version of this documentation (including screenshots and Spanish translation) is available on Notion:

📎 [Notion – Practice #0008 – Exploring Windows Network Command-Line Tools](https://www.notion.so/Practice-0008-2025-10-26-Exploring-Windows-Network-Command-Line-Tools-296eb94034d9800bb6bce142c0af32aa?source=copy_link)

### 🇪🇸 Español

Si lo deseas, puedes leer una versión en español de esta práctica en [Notion](https://www.notion.so/Practice-0008-2025-10-26-Exploring-Windows-Network-Command-Line-Tools-296eb94034d9800bb6bce142c0af32aa?pvs=97#298eb94034d9807dbda9c993eeb4bacc).

