### 1. Web/Application Servers (HTTP/HTTPS & Server-Specific)

| Port(s) | Protocol | Service/Application | Notes |
| --- | --- | --- | --- |
| 80 | TCP | HTTP | Standard web traffic |
| 443 | TCP | HTTPS | Secure web traffic |
| 8000, 8080, 8081 | TCP | Alternate HTTP (Tomcat, Jetty, Jenkins, etc.) | Common for dev servers |
| 8443, 4443 | TCP | Alternate HTTPS (Tomcat SSL, OHS SSL) |  |
| 8005 | TCP | Tomcat Shutdown |  |
| 8009 | TCP | Tomcat AJP Connector |  |
| 8181 | TCP | GlassFish HTTPS |  |
| 4848 | TCP | GlassFish Admin Server |  |
| 9000 | TCP | Jonas Admin Console |  |
| 8008 | TCP | IBM HTTP Server (IHS) Administration |  |
| 9990 | TCP | WildFly Admin Console |  |
| 7001 | TCP | WebLogic Admin Console |  |
| 9060 | TCP | WebSphere (WAS) Admin Console |  |
| 9043 | TCP | WebSphere (WAS) Admin Console (SSL) |  |
| 9080 | TCP | WebSphere JVM HTTP |  |
| 9443 | TCP | WebSphere JVM HTTPS |  |
| 7777 | TCP | Oracle HTTP Server (OHS) |  |
| 1527 | TCP | Apache Derby Network Server |  |

### 2. Control Panels & Hosting Panels

| Port(s) | Protocol | Service | Notes |
| --- | --- | --- | --- |
| 2082 | TCP | cPanel |  |
| 2083 | TCP | cPanel (SSL) |  |
| 2086 | TCP | WHM |  |
| 2087 | TCP | WHM (SSL) |  |
| 2095 | TCP | Webmail |  |
| 2096 | TCP | Webmail (SSL) |  |
| 8880 | TCP | Plesk Control Panel |  |
| 8443 | TCP | Plesk Control Panel (SSL) |  |
| 9998 | TCP | Plesk Windows Webmail (SmarterMail) |  |
| 4643 | TCP | Virtuozzo |  |
| 9001 | TCP | DotNet Panel |  |
| 2077 | TCP | Webdisk |  |
| 2078 | TCP | Webdisk (SSL) |  |
| 2222 | TCP | SFTP (common on shared/reseller) |  |
| 4489 | TCP | RDP (custom in some panels) |  |

### 3. File Transfer Protocols

| Port(s) | Protocol | Service | Notes |
| --- | --- | --- | --- |
| 20, 21, 2121 | TCP | FTP (data & control) | 20 = data (active mode) |
| 989, 990 | TCP | FTPS (implicit SSL) |  |
| 22, 2222 | TCP | SSH / SFTP | Secure file transfer |
| 69 | UDP | TFTP | Trivial FTP |
| 139, 445 | TCP/UDP | CIFS / SMB / Samba | Windows file sharing |
| 514 | TCP | RSYNC (daemon) |  |
| 548 | TCP | AFP (Apple Filing Protocol) |  |
| 1110, 111, 2049, 4045 | TCP | NFS | Network File System |
| 6619 | TCP/UDP | ODette FTP (OFT) | Industrial file transfer |
| 9418 | TCP | Git | Git protocol |
| 9419–9422, 9425 | TCP | MooseFS | Distributed FS |

### 4. Remote Access/Connection

| Port(s) | Protocol | Service | Notes |
| --- | --- | --- | --- |
| 22, 2222 | TCP | SSH | Secure remote shell |
| 23 | TCP | Telnet | Insecure (avoid) |
| 3389 | TCP | RDP | Windows Remote Desktop |
| 5900–5903, 6002–6003 | TCP/UDP | VNC | Remote desktop |

### 5. Directory Services

| Port(s) | Protocol | Service | Notes |
| --- | --- | --- | --- |
| 389 | TCP | LDAP |  |
| 636 | TCP | LDAPS (SSL) | Secure LDAP |

### 6. Databases / Datastores

| Port(s) | Protocol | Service | Notes |
| --- | --- | --- | --- |
| 1433 | TCP | Microsoft SQL Server |  |
| 1521 | TCP | Oracle Listener |  |
| 3306 | TCP | MySQL / MariaDB |  |
| 5432 | TCP | PostgreSQL |  |
| 6379 | TCP | Redis | In-memory store |
| 11211 | TCP | Memcached | Caching |
| 27017 | TCP | MongoDB | NoSQL |
| 50000 | TCP | IBM DB2 |  |

### 7. Messaging & Queue Systems

| Port(s) | Protocol | Service | Notes |
| --- | --- | --- | --- |
| 1364 | TCP/UDP | IBM Connect:Direct |  |
| 1414 | TCP/UDP | IBM MQ Listener |  |
| 7474 | TCP/UDP | Tibco Rendezvous Daemon |  |
| 8200 | TCP/UDP | GoToMyPC |  |
| 15672 | TCP/UDP | RabbitMQ Web UI | Management console |

### 8. Email / Mail Transfer Protocols

| Port(s) | Protocol | Service | Notes |
| --- | --- | --- | --- |
| 25 | TCP/UDP | SMTP | Standard outbound |
| 26, 587 | TCP/UDP | SMTP Alternate | 587 = submission (authenticated) |
| 465 | TCP/UDP | SMTPS (SSL) | Legacy secure SMTP |
| 110 | TCP/UDP | POP3 |  |
| 995 | TCP/UDP | POP3S (SSL) |  |
| 143 | TCP/UDP | IMAP |  |
| 993 | TCP/UDP | IMAPS (SSL) | Secure IMAP |

**Tips for Memorization / Practical Use:**

- Well-known ports (0–1023): Most standard services (e.g., 80, 443, 22, 25).
- Registered ports (1024–49151): Application-specific (e.g., databases).
- Many services can be configured on non-standard ports — always verify in real environments.
- For security: Block unnecessary ports at firewall level; prefer encrypted versions (HTTPS, SSH, etc.).
