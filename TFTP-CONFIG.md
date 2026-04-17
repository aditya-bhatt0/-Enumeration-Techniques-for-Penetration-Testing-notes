Below are **complete, structured notes (no content missed)** from all your images — clean + exam-ready + practical 🔥

---

# 📁 TFTP SERVER SETUP (RHEL / CentOS)

---

# 🧩 1. Package Verification & Installation

### 🔍 Check if TFTP is installed

```bash
rpm -qa | grep tftp
```

👉 Lists installed TFTP packages

---

### 📦 Install TFTP client + server

```bash
yum install tftp tftp-server
```

---

# 📊 2. TFTP Package Information

### ℹ️ Package details

```bash
rpm -qi tftp-server
```

### 📂 List installed files

```bash
rpm -ql tftp-server
```

### ⚙️ Config files

```bash
rpm -qc tftp-server
```

### 📘 Documentation

```bash
rpm -qd tftp-server
```

---

# 📁 3. Directory Setup for TFTP Boot

### 📂 Go to TFTP root directory

```bash
cd /var/lib/tftpboot/
```

---

### 🔍 Check directory exists

```bash
ls -lha /var/lib/ | grep "tftpboot"
```

---

### 📄 Copy test files (logs)

```bash
cp -v /var/log/* /var/lib/tftpboot/
```

---

### ⚠️ Full permission (testing only)

```bash
chmod -R 777 /var/lib/tftpboot/
```

👉 ⚠️ NOT safe for production

---

# ⚙️ 4. Systemd Configuration

### 📁 Copy service files

```bash
cp -v /usr/lib/systemd/system/tftp.service /etc/systemd/system/tftp-server.service
cp -v /usr/lib/systemd/system/tftp.socket /etc/systemd/system/tftp-server.socket
```

---

### 📘 View daemon manual

```bash
man in.tftpd
```

---

# 🔌 5. TFTP Socket File Configuration

### ✏️ Edit socket file

```bash
vim /etc/systemd/system/tftp-server.socket
```

---

### 🧾 Configuration:

```
[Unit]
Description=Tftp Server Activation Socket

[Socket]
ListenDatagram=69
BindIPv6Only=both

[Install]
WantedBy=sockets.target
```

---

# ▶️ 6. Start & Enable TFTP

### 🔄 Restart socket

```bash
systemctl restart tftp-server.socket
```

---

### 🚀 Enable service on boot

```bash
systemctl enable tftp-server
```

---

# 🔥 7. Firewall Configuration (firewalld)

### 🔍 Check firewall

```bash
systemctl status firewalld
```

---

### ▶️ Start firewall

```bash
systemctl start firewalld
systemctl enable firewalld
```

---

### ➕ Allow TFTP service

```bash
firewall-cmd --zone=public --add-service=tftp --permanent
```

---

### ➕ OR open port manually

```bash
firewall-cmd --zone=public --add-port=69/udp --permanent
```

---

### 🔄 Reload firewall

```bash
firewall-cmd --reload
```

---

### ✅ Verify rules

```bash
firewall-cmd --zone=public --list-all
```

---

# 📡 8. Verify TFTP Server

### Using netstat

```bash
netstat -nltup | grep 69
```

---

### Using ss

```bash
ss -uln | grep 69
```

---

### ✅ Expected Output

👉 `in.tftpd` listening on:

* `0.0.0.0:69`
* `[::]:69`

---

# 🌐 9. What is TFTP?

👉 **TFTP (Trivial File Transfer Protocol)**
Lightweight file transfer protocol

---

# 🔑 Key Characteristics

* Uses **UDP (connectionless)**
* Default port: **69/UDP**
* ❌ No authentication
* ❌ No directory listing
* Simple design

---

### 📌 Used For:

* Firmware transfer
* Configuration backup
* PXE boot files

---

# ⚠️ Security Note

👉 TFTP is **insecure**

* No encryption
* No authentication
* Easily exploitable

---

# 🎯 10. TFTP Enumeration (Pentesting)

---

# 🎯 Objectives

* Download sensitive files (configs, creds)
* Upload malicious files (web shell)
* Find firmware / boot files
* Map internal network

---

# 🛠️ 11. Enumeration Tools & Commands

---

## 🔍 Nmap UDP Scan

```bash
nmap -v -sU -p 69 <target-ip>
```

---

## 🔎 Detect Version

```bash
nmap -sU -p 69 --script=tftp-version <target-ip>
```

---

## 📂 File Enumeration

```bash
nmap -sU -p 69 --script=tftp-enum <target-ip>
```

---

## 📜 Custom Wordlist

```bash
nmap -sU -p 69 --script=tftp-enum \
--script-args tftp-enum.filelist=customlist.txt <target-ip>
```

---

# 💻 12. Manual TFTP Access

```bash
tftp 192.168.10.106
```

---

### Enable verbose

```bash
verbose
```

---

### 📥 Upload file

```bash
put shell.php
```

👉 Example:

```
putting shell.php to 192.168.10.106:shell.php
Sent successfully
```

---

# ⚠️ Exploit Insight

👉 If upload works:

* Write access enabled ❌
* Vulnerable system
* Can lead to **web shell / RCE**

---

# 🌐 13. Web Directory Brute Force (Different Case)

### 🔎 Feroxbuster

```bash
feroxbuster -u http://192.168.10.106 \
-w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt \
-t 500
```

---

# ⚠️ Important Difference

| Feature          | TFTP | Web Server |
| ---------------- | ---- | ---------- |
| Protocol         | UDP  | HTTP       |
| Port             | 69   | 80/443     |
| Dir brute force  | ❌    | ✅          |
| File brute force | ✅    | ✅          |

---

# 🧠 Final Concept

👉 **TFTP = File Guessing Attack**
👉 **Web = Directory Brute Force**

---

# 🎯 One-Line Exam Answer

👉 *TFTP enumeration is done by brute-forcing file names over UDP port 69, while directory brute force tools like dirsearch are used only for HTTP-based web servers.*

---

If you want next 🔥
I can convert this into:

* 📄 PDF notes (A4 exam format)
* 🧠 Mind map
* 🧪 Full lab (Kali ↔ CentOS attack simulation)

Just tell 👍
