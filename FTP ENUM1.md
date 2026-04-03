

---

# 🛠️ FTP ENUMERATION (File Transfer Protocol)

## 📌 Definition

👉 FTP Enumeration is the process of discovering information about an FTP service such as:

* Service version
* User access (anonymous or authenticated)
* File structure
* Permissions
* Potential vulnerabilities

💡 This is a key step in **reconnaissance in penetration testing**

---

# 🔍 1. BANNER GRABBING

## 🎯 Goal

👉 Identify FTP service and version

---

## 🧰 Using Netcat

```bash
nc <target_ip> 21
nc -v 192.168.1.207 21
nc -v 192.168.1.207 2121
```

---

## 🧰 Using Telnet

```bash
telnet <target_ip> 21
telnet 192.168.1.207 21
telnet 192.168.1.207 2121
```

---

# 🔎 2. FTP VERSION DETECTION

## 🧰 Nmap Scan

```bash
nmap -v -sV -sT -sC -A -p 21,2121 192.168.1.207
```

---

## 🔁 Scan FTP Ports Using Wildcard

```bash
nmap -v -p ftp* -sT -sV -A 192.168.1.207
```

---

## 📜 Run Specific NSE Script

```bash
nmap -v -sT -p 21 --script=ftp-syst.nse 192.168.1.207
```

---

## 📦 Run All FTP Scripts

```bash
nmap -v -sT -sV -A -O -p 21,2121 --script=ftp* 192.168.1.250
```

---

# 🔐 3. ANONYMOUS LOGIN CHECK

## 🔑 Common Credentials

| Username  | Password                                            |
| --------- | --------------------------------------------------- |
| anonymous | [anonymous@domain.com](mailto:anonymous@domain.com) |
| ftp       | [ftp@domain.com](mailto:ftp@domain.com)             |
| guest     | [guest@domain.com](mailto:guest@domain.com)         |

---

## 🧪 Manual Login

```bash
ftp <target_ip>
```

### Example:

```
Name: anonymous
Password: anonymous@domain.com
```

---

## ✅ Success Indicator

```bash
230 Login successful
```

---

## 🧰 Using Nmap

```bash
nmap -p 21 --script=ftp-anon <target_ip>
```

---

## 📥 Download Files

```bash
wget -m ftp://anonymous@192.168.1.207
wget -m --no-passive ftp://anonymous@192.168.1.207
```

---

## 🔍 File Analysis

```bash
file filename
strings filename
cat filename
```

---

# 📁 4. DIRECTORY TRAVERSAL

## 🎯 Goal

👉 Access restricted directories

---

## 🔧 Key Commands

```bash
ls -al
pwd
cd ..
cd /etc
cd ../../../../etc
```

---

## 🚨 Indicators of Vulnerability

* Access outside FTP root
* Access to sensitive files (`/etc/passwd`)

---

# 📚 5. FTP COMMANDS REFERENCE

| Command | Description             |
| ------- | ----------------------- |
| ls      | List files              |
| cd      | Change directory        |
| pwd     | Show current directory  |
| get     | Download file           |
| put     | Upload file             |
| delete  | Remove file             |
| mget    | Download multiple files |

---

# 🔄 6. FTP TRANSFER MODES

## 📝 ASCII Mode

```bash
ascii
```

👉 Text files only

---

## 📦 Binary Mode (Recommended)

```bash
binary
```

👉 Safe for all file types

---

# 🔌 7. FTP CONNECTION MODES

## 🔁 Active Mode

```bash
quote PORT
```

---

## ✅ Passive Mode (Preferred)

```bash
passive
```

---

# 📝 8. WRITABLE DIRECTORY CHECK

## 🧪 Test Write Permissions

```bash
mkdir test
cd test
put test.txt
delete test.txt
rmdir test
```

---

## 🚨 Indicators

* File upload allowed
* Directory creation allowed

---

# 🔓 9. BRUTE-FORCING FTP CREDENTIALS

## 🧰 Hydra

```bash
hydra -l user -P passwords.txt ftp://192.168.1.207
hydra -L users.txt -P passwords.txt ftp://192.168.1.207
hydra -C creds.txt ftp://192.168.1.207
```

---

## 🧰 Medusa

```bash
medusa -h 192.168.1.207 -u user -P passwords.txt -M ftp
```

---

## 🧰 Ncrack

```bash
ncrack -p 21 -U users.txt -P passwords.txt 192.168.1.207
```

---

# 💣 10. METASPLOIT MODULES

```bash
msfconsole
```

## 📦 Modules

```bash
use auxiliary/scanner/ftp/ftp_version
use auxiliary/scanner/ftp/anonymous
use auxiliary/scanner/ftp/ftp_login
```

---

# 🧰 11. USEFUL NMAP SCRIPTS

```bash
nmap -sV -p21 --script=ftp-anon,ftp-syst,ftp-bounce,ftp-vsftpd-backdoor <target_ip>
```

---

# 🚨 12. COMMON VULNERABILITIES

* Anonymous login enabled
* Weak credentials
* Writable directories
* FTP bounce attack
* Outdated FTP software
* Clear-text communication

---

# 🛡️ 13. MITIGATION BEST PRACTICES

* Disable anonymous access
* Use strong passwords
* Enable account lockout
* Restrict access by IP
* Use **FTPS/SFTP instead of FTP**
* Monitor logs regularly

---

# 🚀 ONE-LINER ENUMERATION

```bash
nmap -sV -p21 --script="ftp-*" <target_ip>
```

---


