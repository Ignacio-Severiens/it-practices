# Practice #0009 – Managing and Testing Windows Defender Firewall Rules

### 🎯 Goal

Understand how to view, configure, and manage **Windows Defender Firewall** settings — including creating, enabling, and disabling custom inbound rules — to control traffic and secure a Windows system.

---

### 🔧 Tools and Materials
- Windows 11 VM (from [Practice #0005](https://github.com/Ignacio-Severiens/it-practices/tree/main/practice-0005-virtualbox-win11))
- Windows Defender Firewall (GUI)
- Administrator privileges
- A local web server (e.g., `python -m http.server 80`) to test the blocking rule

---

## ⚙️ Section 1 – Viewing Windows Firewall Settings

**Steps:**
1. Open the Control Panel → System and Security → Windows Defender Firewall.  
2. Review the three network profiles:
   - **Domain** → Not available because this VM is not part of a domain.  
   - **Private**  
   - **Public**  
3. Verify the firewall is enabled for all profiles.

📝 *Note:* Each profile maintains its own rule set and should always be active unless troubleshooting.

📸 *Screenshot:* `01-windows-defender-firewall.png`

---

## 🔒 Section 2 – Creating a Custom Inbound Rule

**Goal:** Block inbound connections to **port 80 (HTTP)** — simulating how to protect a system from unauthorized unencrypted web traffic.

**Steps:**
1. Open **Windows Defender Firewall with Advanced Security** (`wf.msc`).  
2. Navigate to **Inbound Rules → New Rule...**  
3. Select **Port → TCP → Specific local ports: 80**  
4. Choose **Block the connection**  
5. Apply to **all profiles** (Domain, Private, Public).  
6. Name the rule:  
   ```
   Block inbound HTTP (Port 80)
   ```

**Result:** The rule appears under *Inbound Rules* with a red “no entry” icon.

📸 *Screenshots:*  
- `02-windows-defender-firewall-advanced-security.png`  
- `03-new-inbound-rule-wizard.png`  
- `04-new-inbound-rule-created.png`

---

## 🧪 Section 3 – Testing the Rule

If you have Python installed:

```
python -m http.server 80
```

→ Starts a simple web server on port 80, accessible from other devices in the same network.

📸 *Screenshot:* `05-python-server-port80.png`

From another device or VM in the same network, open:

```
http://<VM_IP>
```

*(Use `ipconfig` in the VM to verify the IPv4 address.)*

### 🧠 Result

- **Firewall rule disabled →** Page loads normally:  
  - `06-successful-connection.png`  
  - `07-successful-connection-vm.png`  

- **Firewall rule enabled →** Connection blocked:  
  - `08-blocked-connection.png`

---

## 📚 Learned

- How to navigate **Windows Defender Firewall** basic and advanced settings  
- Difference between **inbound**, **outbound**, and **connection security** rules  
- How to **block specific ports** and confirm their effect  
- How to simulate **traffic filtering** and test rule behavior in a VM environment  
- Importance of **testing changes safely** and **documenting configurations**

---

## ✅ Final Status

Rule created, tested, enabled/disabled, and verified. Screenshots captured.

---

## 📘 Extended Version

A more detailed version of this documentation (including screenshots and Spanish translation) is available on Notion:

📎 [Notion – Practice #0009 – Managing and Testing Windows Defender Firewall Rules](https://www.notion.so/Practice-0009-2025-10-30-Managing-and-Testing-Windows-Defender-Firewall-Rules-29beb94034d98014b602f096a8af6d25?source=copy_link)

### 🇪🇸 Español

Si lo deseas, puedes leer una versión en español de esta práctica en [Notion](https://www.notion.so/Practice-0009-2025-10-30-Managing-and-Testing-Windows-Defender-Firewall-Rules-29beb94034d98014b602f096a8af6d25?pvs=97#29beb94034d9806f93e5ca1a7eac59d8).
