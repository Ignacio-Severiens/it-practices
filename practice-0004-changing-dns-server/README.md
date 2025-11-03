# Practice #0004 – Changing my DNS server

## 🎯 Goal

Replace the default ISP-provided DNS server with a more private and secure one.

---

## 🔧 Tools and Materials

- Ubuntu 24.04 LTS
- NetworkManager
- Terminal

---

## 📘 Commands Used

| Command | Description |
| ------- | ----------- |
| `nmcli dev show ¦ grep DNS` | Shows current DNS configuration |
| `nmcli con mod "<connection-name>" ipv4.dns "<dns1> <dns2>"` | Sets IPv4 DNS servers |
| `nmcli con mod "<connection-name>" ipv4.ignore-auto-dns yes` | Ignores automatic DNS from DHCP |
| `nmcli con mod "<connection-name>" ipv6.dns "<dns1> <dns2>"` | Sets IPv6 DNS servers |
| `nmcli con mod "<connection-name>" ipv6.ignore-auto-dns yes` | Ignores automatic IPv6 DNS from DHCP |
| `nmcli con up "<connection-name>"` | Reactivates the connection to apply changes |

> _Note: The pipe character (`|`) is a column separator in Markdown tables and breaks formatting inside table cells.  
> To work around this, the pipe in commands inside tables is replaced by the similar-looking “broken bar” (`¦`).  
> If you copy commands from this table, replace `¦` with `|` before running._

---

## 📝 Steps Performed

1. **Identified the active connection** with:
   ```bash
   nmcli con show
   ```

2. **Checked current DNS configuration**  
   ```bash
   nmcli dev show | grep DNS
   ```

3. **Set new IPv4 DNS servers** (Quad9 in this case)  
   ```bash
   nmcli con mod "<connection-name>" ipv4.dns "9.9.9.9 149.112.112.112"
   nmcli con mod "<connection-name>" ipv4.ignore-auto-dns yes
   ```

4. **Set new IPv6 DNS servers** (Quad9)  
   ```bash
   nmcli con mod "<connection-name>" ipv6.dns "2620:fe::fe 2620:fe::9"
   nmcli con mod "<connection-name>" ipv6.ignore-auto-dns yes
   ```

5. **Applied changes**  
   ```bash
   nmcli con up "<connection-name>"
   ```

6. **Verified configuration**:
   ```bash
   resolvectl status
   ```

7. **Rebooted to confirm persistence** — settings remained correct.

8. **Verified connectivity**:
    ```bash
    ping -c 4 dnsleaktest.com
    ```

---

## 💡 Problems Encountered (and Solutions)

| Problem | Solution |
| ------- | -------- |
| Changes didn't apply immediately | Used `nmcli con up` to reapply |
| DNS reverted to default | Set `ignore-auto-dns yes` for both IPv4 and IPv6 |
| Initially forgot IPv6 DNS | Applied same method as IPv4 with `ipv6.dns` and `ipv6.ignore-auto-dns` |

---

## 📚 What I Learned

- How to set custom DNS servers for both IPv4 and IPv6 in NetworkManager.
- That ISPs can intercept and override DNS queries unless encryption is used.
- The importance of verifying changes with external DNS leak tests.

---

## ✅ Final Status

✅ DNS servers successfully changed in NetworkManager, but ISP-level interception detected.

---

## 📘 Extended Version

A more detailed version of this documentation (including screenshots and Spanish translation) is available on Notion:

📎 [Notion – Practice #0004 – Changing my DNS server](https://www.notion.so/Practice-0004-2025-08-10-Changing-my-DNS-server-248eb94034d9807382b4fa9e0d192ac5)
