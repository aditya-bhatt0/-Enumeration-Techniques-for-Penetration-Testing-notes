

# 📌 1. SMB ENUMERATION (Concept)

### 🧠 What is SMB?

- SMB (Server Message Block) → file sharing protocol (Windows)
- Used for:
    - 📁 File sharing
    - 🖨️ Printers
    - 🔗 Network services
- **High-value target in Pentesting**

---

# 🌐 2. Common SMB Ports

| Protocol | Port | Description |
| --- | --- | --- |
| TCP | 445 | Direct SMB communication |
| TCP | 139 | NetBIOS Session Service |
| TCP | 135 | RPC (used with SMB) |
| UDP | 137 | NetBIOS Name Service |

---

# 🎯 3. Why SMB Enumeration

👉 Critical phase in pentesting because it reveals:

- Credential harvesting
- Privilege escalation paths
- Lateral movement opportunities
- Full Active Directory compromise

---

# 🎯 4. Key Objectives

## 🔍 Identify Shared Resources

- Discover accessible shares
- Hidden/admin shares:
    - `C$`
    - `ADMIN$`
    - `IPC$`

---

## 👥 Enumerate Users & Groups

- Extract valid usernames
- Identify privileged groups
- Helps in:
    - Password spraying
    - Brute-force attacks

---

## 🖥️ Gather Host Information

- OS version
- Domain / Workgroup
- Hostname (NetBIOS)

---

## ⚠️ Identify Misconfigurations

- Anonymous (null session) access
- Weak share permissions
- Misconfigured service accounts

---

## 🔐 Extract Security Policies

- Password policy
- Account lockout settings

---

# 💥 5. Real-World Impact

| Misconfiguration | Impact |
| --- | --- |
| Anonymous SMB access | Information disclosure + user enumeration |
| Weak NTLM authentication | NTLM relay + brute-force |
| Over-permissive shares | Data leakage + privilege escalation |
| Accessible SAM/LSA | Credential dumping |

---

# 🛠️ 6. Tools for SMB Enumeration

| Tool | Use |
| --- | --- |
| nmap | Port scan + SMB scripts |
| smbclient | Access shares |
| enum4linux | Legacy enumeration |
| enum4linux-ng | Advanced enumeration |
| smbmap | Share permission mapping |
| crackmapexec / netexec (nxc) | Advanced enumeration + exploitation |
| rpcclient | RPC-based enumeration |
| impacket tools | Advanced SMB/RPC operations |

---

# ⚡ 7. Enumeration Techniques (Commands)

---

## 🔍 Nmap SMB Scan

```bash
nmap -p 445 --script smb-enum* <target>
```

```bash
nmap -v -sT -sV -p 445 --script smb-enum* 192.168.10.52
```

---

## 🎯 Targeted SMB Scripts

```bash
nmap -p 445 --script smb-os-discovery,smb-enum-shares,smb-enum-users <target>
```

```bash
nmap -v -sT -sV -p 445 --script smb-os-discovery,smb-enum-shares,smb-enum-users 192.168.10.52
```

---

## 📂 List Shares (Anonymous)

```bash
smbclient -L //<target> -N
```

```bash
smbclient -L //192.168.10.52 -N
```

---

## 📁 Access Share

```bash
smbclient //<target>/<sharename> -N
```

---

## 📊 smbmap

```bash
smbmap -H <target>
```

---

## 🧪 enum4linux

```bash
enum4linux -a <target>
```

---

## 🚀 enum4linux-ng

```bash
enum4linux-ng -A <target>
```

---

## ⚡ NetExec (NXC)

```bash
nxc smb <target> -u '' -p '' --shares
```

```bash
nxc smb <target> -u '' -p '' --users
```

---

# 🔐 8. SMB Signing Check

```bash
nmap --script smb2-security-mode -p 445 <target>
```

---

# ⚠️ 9. Common Weaknesses to Check

- SMBv1 enabled
- Null session allowed
- SMB signing disabled
- Weak share permissions
- Credential reuse across systems

---

# 🛡️ 10. Defensive Recommendations

- Disable SMBv1
- Enforce SMB signing
- Restrict anonymous access
- Apply least privilege
- Monitor SMB logs
- Strong password policy

---

# 🔍 11. SMB VERSION DETECTION (VERY IMPORTANT)

---

## 🚀 🔴 FIRST STEP (ADD THIS — YOUR LINE)

👉 **Always start with Nmap to detect SMB version before deeper analysis**

```bash
nmap -v -sV -p 445,139 <target>
```

---

## 🌐 Port Checking (Basic Connectivity)

```bash
nc -v 192.168.1.174 139
```

```bash
nc -vvv 192.168.1.174 139
```

```bash
telnet 192.168.1.174 139
```

---

## 🔎 Nmap Service & Version Detection

```bash
nmap -v -sV -sT -sC -A -p 445,139 192.168.1.174
```

### Options:

- `sV` → Service version detection
- `sC` → Default scripts
- `A` → Aggressive (OS + scripts + version)

---

## 🎯 Targeted SMB Version Scripts

```bash
nmap -p 445 --script smb-protocols,smb-os-discovery 192.168.1.174
```

```bash
nmap -v -sT -sV -A -p 139,445 --script smb-protocols,smb-os-discovery 192.168.10.134
```

---

# 📡 12. Tcpdump SMB Version Fingerprinting

👉 Used when:

- Tools fail
- Need deeper packet-level analysis

---

## 📌 Capture SMB Traffic (Port 139)

```bash
tcpdump -s0 -n -i eth0 src 192.168.1.174 and port 139 -A -c 10 2>/dev/null | grep -i "samba\|s.a.m"
```

---

## 📌 Capture SMB Traffic (Port 445)

```bash
tcpdump -s0 -n -i eth0 src 192.168.1.174 and port 445 -A -c 10 2>/dev/null | grep -i "samba\|s.a.m"
```

---

# 🔁 Tcpdump + smbclient Trigger

👉 Improves detection reliability

### Terminal 1:

```bash
tcpdump -s0 -n -i eth0 src 192.168.1.174 and port 139 -A -c 10 2>/dev/null | grep -i "samba\|s.a.m"
```

### Terminal 2:

```bash
smbclient -L //192.168.1.174 -N
```

---

# 🧠 IMPORTANT LINE (ADD IN NOTES)

👉 **Tcpdump and Wireshark are used when automated tools fail; they allow manual packet analysis to identify SMB version and server details from network responses.**

---

# 🔥 FINAL EXAM LINE

👉 **SMB enumeration identifies shares, users, and misconfigurations, while version detection helps map vulnerabilities like SMBv1 (EternalBlue), enabling attackers to plan exploitation.**

---

If you want next:

- 🔴 Exploitation (EternalBlue, null session attack)
- 🟢 Full defensive hardening checklist (enterprise level)
- ⚡ Automated script for SMB enum (Python / Bash)

Just tell 👍
