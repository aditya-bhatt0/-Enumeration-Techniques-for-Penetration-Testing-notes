## 

---

## 🎯 **Purpose**

- Intentionally weaken Windows security **ONLY for lab / penetration testing**
- Simulates:
    - Anonymous access (null session)
    - SMB misconfigurations
    - Weak enterprise environments

⚠️ **Warning:** Never apply in production

---

## ⚙️ **Step 1: Open Group Policy Editor**

```bash
Win + R → gpedit.msc → Enter
```

### 📂 Navigate Path:

```
Computer Configuration
 └── Policies
     └── Windows Settings
         └── Security Settings
             └── Local Policies
                 └── Security Options
```

---

## 🔐 **Step 2: Security Settings to Modify**

| Policy | Configuration |
| --- | --- |
| Accounts: Guest account status | ✅ Enabled |
| Network access: Let Everyone permissions apply to anonymous users | ✅ Enabled |
| Network access: Allow anonymous SID/Name translation | ✅ Enabled |
| Network access: Restrict anonymous access to Named Pipes and Shares | ❌ Disabled |
| Network access: Do not allow anonymous enumeration of SAM accounts | ❌ Disabled |
| Network access: Do not allow anonymous enumeration of SAM accounts and shares | ❌ Disabled |
| Network access: Shares that can be accessed anonymously | `IPC$` |

---

## ⚡ **Apply Changes**

```bash
gpupdate /force
```

👉 **Final Line (Important):**

✅ **System is now vulnerable**

---

## 💡 **What These Changes Do (Core Concept)**

- Enables **anonymous (unauthenticated) access**
- Allows enumeration of:
    - 🧑‍💻 SAM accounts
    - 📁 Network shares
- Activates **Guest account**
- Exposes **IPC$ share** (used in null session attacks)

---

## 🖥️ **Step 3: PowerShell SMB Vulnerable Setup**

👉 Run **PowerShell as Administrator**

---

### 🔓 Allow Anonymous Enumeration

```bash
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v RestrictAnonymous /t REG_DWORD /d 0 /f
```

---

### 📡 Allow Anonymous Access to IPC$

```bash
reg add "HKLM\SYSTEM\CurrentControlSet\Services\LanManServer\Parameters" /v NullSessionShares /t REG_MULTI_SZ /d "IPC$" /f
```

---

### ⚠️ Enable SMBv1 (Highly Insecure)

```bash
Enable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol -NoRestart
```

---

### 🔄 Restart SMB Service

```bash
Restart-Service LanmanServer -Force
```

---

### 🔁 Apply Policy Again

```bash
gpupdate /force
```

👉 **Final Line (Important):**

✅ **System is now vulnerable**

---

## 📁 **Step 4: Create Vulnerable SMB Share**

---

### 📂 Create Directory

```bash
New-Item -Path "C:\VulnShare" -ItemType Directory
```

---

### 🌐 Create SMB Share

```bash
New-SmbShare -Name "VulnShare" -Path "C:\VulnShare" -FullAccess "Everyone"
```

---

### 🔓 Set Full NTFS Permissions

```bash
icacls "C:\VulnShare" /grant Everyone:F /T
```

👉 **Final Line (Important):**

✅ **System is now vulnerable**

---

## 🧠 **Attack Surface Created (Very Important for Exams)**

| Feature | Risk |
| --- | --- |
| Guest Account Enabled | Unauthorized login |
| Anonymous Enumeration | User + share discovery |
| IPC$ Exposure | Null session attack |
| SMBv1 Enabled | EternalBlue / Worm attacks |
| Everyone Full Access | Data theft / ransomware |

---

## 🔍 **Real-World Use (VA / PT)**

- Used in:
    - Vulnerability Assessment labs
    - Red Team simulations
    - SMB exploitation practice

### Tools that exploit this:

- `enum4linux`
- `smbclient`
- `rpcclient`
- `nmap --script smb-enum*`
- Metasploit SMB modules

---

## 🧾 **One-Line Summary (Exam Ready)**

👉 **This setup disables key Windows security controls to allow anonymous SMB access, enabling enumeration of users, shares, and system resources, making the system vulnerable to network-based attacks.**

---
