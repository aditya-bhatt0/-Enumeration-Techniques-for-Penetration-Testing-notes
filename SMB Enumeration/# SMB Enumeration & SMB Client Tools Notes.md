# SMB Nmap Enumeration (Nmap Scripts)

## Overview

SMB (Server Message Block) is a network file-sharing protocol used mainly in Windows environments.

SMB Enumeration helps identify:

- Users
- Shared folders
- System details
- Running services
- Security settings
- Domain information
- Potential vulnerabilities

---

# Basic SMB Scan

## Purpose

Performs:

- TCP Connect Scan
- Service Version Detection
- Default NSE Scripts

## Command

```bash
nmap -v -sT -sV -sC -p 135,137,139,445 192.168.1.52
```

## Explanation

| Option | Meaning |
| --- | --- |
| `-v` | Verbose output |
| `-sT` | TCP Connect Scan |
| `-sV` | Detect service versions |
| `-sC` | Run default NSE scripts |
| `-p` | Specify ports |

## Important SMB Ports

| Port | Service |
| --- | --- |
| 135 | MSRPC |
| 137 | NetBIOS Name Service |
| 139 | NetBIOS Session Service |
| 445 | SMB over TCP |

---

# Targeted SMB NSE Scripts

# 1. OS Detection

## Purpose

Identifies:

- Operating System
- SMB Version

## Command

```bash
nmap -v -sT -sV --script=smb-os-discovery.nse -p 139,445 192.168.1.52
```

---

# 2. System Information

## Purpose

Retrieves detailed host information via SMB.

## Command

```bash
nmap -v -sT -sV --script=smb-system-info.nse -p 135,137,139,445 192.168.1.52
```

---

# 3. Security Mode

## Purpose

Checks:

- Authentication Level
- SMB Signing
- Security Configuration

## Command

```bash
nmap -v -sT -sV --script=smb-security-mode.nse -p 135,137,139,445 192.168.1.52
```

---

# 4. Share Enumeration

## Purpose

Lists available SMB shares.

## Command

```bash
nmap -v -sT -sV --script=smb-enum-shares.nse -p 135,137,139,445 192.168.1.52
```

---

# 5. Share Browsing

## Purpose

Enumerates:

- SMB Shares
- Directory Contents

## Command

```bash
nmap -v -sT -sV --script=smb-enum-shares,smb-ls -p 135,137,139,445 192.168.1.52
```

---

# 6. User Enumeration

## Purpose

Lists SMB users.

## Command

```bash
nmap -v -sT -sV --script=smb-enum-users.nse -p 135,137,139,445 192.168.1.52
```

---

# 7. Domain Enumeration

## Purpose

Retrieves domain-related information.

## Command

```bash
nmap -v -sT -sV --script=smb-enum-domains.nse -p 135,137,139,445 192.168.1.52
```

---

# 8. Group Enumeration

## Purpose

Lists SMB groups.

## Command

```bash
nmap -v -sT -sV --script=smb-enum-groups.nse -p 135,137,139,445 192.168.1.52
```

---

# 9. Process Enumeration

## Purpose

Lists running processes via SMB.

## Command

```bash
nmap -v -sT -sV --script=smb-enum-processes.nse -p 135,137,139,445 192.168.1.52
```

---

# 10. Service Enumeration

## Purpose

Lists services running on the target system.

## Command

```bash
nmap -v -sT -sV --script=smb-enum-services.nse -p 135,137,139,445 192.168.1.52
```

---

# 11. Session Enumeration

## Purpose

Displays active SMB sessions.

## Command

```bash
nmap -v -sT -sV --script=smb-enum-sessions.nse -p 135,137,139,445 192.168.1.52
```

---

# Full SMB Enumeration

# Run All SMB Enumeration Scripts

## Command

```bash
nmap -v -sT -sV --script=smb-enum* -p 135,137,139,445 192.168.1.52
```

---

# Full Enumeration with Directory Listing

## Command

```bash
nmap -v -sT -sV --script=smb-enum*,smb-ls.nse -p 135,137,139,445 192.168.1.52
```

---

# Authenticated Enumeration

## Purpose

Uses valid credentials for deeper enumeration.

## Command

```bash
nmap -v -sT -sV --script=smb-enum-shares.nse \
--script-args smbuser=admin,smbpass='P@ssw0rd' \
-p 135,137,139,445 192.168.1.52
```

---

# Script Discovery & Help

# List SMB NSE Scripts

```bash
ls /usr/share/nmap/scripts/smb*
```

# View Help for Specific Script

```bash
nmap --script-help=smb-enum-users
```

---

# SMB Client Tools

## Overview

SMB client tools interact with SMB services for:

- Enumeration
- File access
- Upload/download
- Remote administration

---

# smbclient

## Purpose

SMB interaction tool similar to FTP client.

---

# Share Enumeration

## List Available Shares

```bash
smbclient -L //192.168.1.52
```

## List Shares with Authentication

```bash
smbclient -L //192.168.1.52 -U administrator
```

## Attempt Anonymous Access

```bash
smbclient -N -L //192.168.1.52
```

| Option | Meaning |
| --- | --- |
| `-L` | List shares |
| `-U` | Username |
| `-N` | Null/anonymous login |

---

# Accessing Shares

# Access Shared Folder

```bash
smbclient //192.168.1.52/VulnShare
```

# Anonymous Access

```bash
smbclient -N //192.168.10.52/VulnShare
```

# Access Administrative Shares

## C$ Share

```bash
smbclient //192.168.1.52/C$ -U administrator
```

## ADMIN$ Share

```bash
smbclient //192.168.1.52/ADMIN$ -U administrator
```

---

# Authentication Methods

# Prompt for Password

```bash
smbclient //192.168.1.52/C$ -U administrator
```

# Inline Credentials

```bash
smbclient -U 'administrator%@rmour123' //192.168.1.52/C$
```

# List Shares Using Credentials

```bash
smbclient -L 192.168.1.52 -U 'administrator%@rmour123'
```

---

# Pass-the-Hash

## Authenticate Using NTLM Hash

```bash
smbclient //192.168.1.52/C$ -U administrator --pw-nt-hash E41B0D190802C1FCFD3D8DFF20D97090
```

---

# Interactive Commands

## After Connecting to Share

# Change Local Directory

```bash
lcd /tmp/
```

# List Files

```bash
ls
```

# Change Directory

```bash
cd dir1
```

# Print Working Directory

```bash
pwd
```

# Download File

```bash
get file.txt
```

# Download Multiple Files

```bash
mget *.exe
```

# Upload File

```bash
put file.txt
```

# Upload Multiple Files

```bash
mput *.exe
```

# Create Directory

```bash
mkdir new_folder
```

---

# impacket-smbclient

## Overview

Part of Impacket toolkit for advanced SMB interaction and authentication.

---

# Help

```bash
impacket-smbclient -h
```

---

# Connection

# Username & Password Authentication

```bash
impacket-smbclient administrator:@rmour123@192.168.1.52
```

# Domain Authentication

```bash
impacket-smbclient armour.com/administrator:@rmour123@192.168.1.52
```

---

# smbget

## Purpose

Downloads files from SMB shares like `wget`.

---

# Download File

```bash
smbget smb://192.168.1.52/data/file.exe
```

# Download with Authentication

```bash
smbget -U administrator smb://192.168.1.52/data/file.zip
```

# Save to Specific Location

```bash
smbget -U administrator smb://192.168.1.52/data/file.zip -o /tmp/file.zip
```

---

# smbtar

## Purpose

Archives SMB shares into TAR files.

---

# Create Archive from Share

```bash
smbtar -s 192.168.1.52 -x data -t data.tar -v
```

# Create Archive with Authentication

```bash
smbtar -s 192.168.1.52 -x C$ -t data.tar -u administrator -p password -v
```

---

# Quick SMB Enumeration Workflow

## Step 1 — Basic Scan

```bash
nmap -sV -sC -p 139,445 TARGET
```

## Step 2 — Enumerate Shares

```bash
nmap --script=smb-enum-shares TARGET
```

## Step 3 — Enumerate Users

```bash
nmap --script=smb-enum-users TARGET
```

## Step 4 — Try Anonymous Access

```bash
smbclient -N -L //TARGET
```

## Step 5 — Access Shares

```bash
smbclient //TARGET/share
```

## Step 6 — Download Files

```bash
get file.txt
```

---

# Important SMB Shares

| Share | Purpose |
| --- | --- |
| `C$` | Administrative access to C drive |
| `ADMIN$` | Windows admin share |
| `IPC$` | Inter-process communication |
| `NETLOGON` | Domain login scripts |
| `SYSVOL` | Group policy files |

---

# Important Notes

## SMBv1 Risks

- Vulnerable to EternalBlue
- Insecure and outdated
- Should be disabled

## SMB Signing

- Prevents tampering
- If disabled → vulnerable to relay attacks

## Null Sessions

Anonymous SMB access:

```bash
smbclient -N -L //TARGET
```

Can leak:

- Usernames
- Shares
- Domain info

---

# Common NSE SMB Scripts

| Script | Purpose |
| --- | --- |
| `smb-os-discovery` | OS information |
| `smb-system-info` | Host details |
| `smb-security-mode` | Security configuration |
| `smb-enum-shares` | List shares |
| `smb-enum-users` | List users |
| `smb-enum-groups` | List groups |
| `smb-enum-domains` | Domain information |
| `smb-enum-services` | Running services |
| `smb-enum-sessions` | Active sessions |
| `smb-ls` | List directory contents |
