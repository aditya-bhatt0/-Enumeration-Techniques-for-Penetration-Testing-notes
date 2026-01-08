**Notes:**

msfconsole is the primary console interface for the Metasploit Framework. It provides access to most features, including a stable, feature-rich environment with readline support, tab completion, and the ability to execute external shell commands. It is the recommended and most supported way to interact with Metasploit.

## msfconsole Usage and Commands

### Default Installation Path

Shows the default installation path of the Metasploit Framework on a typical Linux system:

```bash
cd /usr/share/metasploit-framework/

```

**Notes:**

This directory contains the core files, modules, and configuration for Metasploit.

### Install Metasploit Framework

Installs the Metasploit Framework package using the system package manager (Debian/Ubuntu example):

```bash
apt update

```

```bash
apt install metasploit-framework

```

**Notes:**

- Update package lists first with `apt update`.
- For RPM-based systems (e.g., CentOS), use `yum install metasploit-framework` or download from Rapid7's official installer.

### Manage PostgreSQL Service

Starts and enables the PostgreSQL service, which is required for Metasploit's database features:

```bash
systemctl start postgresql.service

```

```bash
systemctl enable postgresql.service

```

**Notes:**

- PostgreSQL stores hosts, services, vulnerabilities, and loot.
- Verify installation with `systemctl status postgresql.service`.

### Verify PostgreSQL is Running

Verifies that PostgreSQL is running and listening on the expected ports using netstat or ss:

```bash
netstat -nltup | grep postgres

```

```bash
ss -nltup | grep postgres

```

**Notes:**

- Expected output: PostgreSQL listening on TCP port 5432.
- Install `net-tools` if netstat is unavailable: `apt install net-tools`.

### Manage and Initialize Metasploit Database

Manages and initializes the Metasploit database using msfdb:

```bash
msfdb

```

```bash
msfdb init

```

**Notes:**

- `msfdb init` sets up the database schema and default user (`msf` with a generated password).
- Run as root or with sudo if needed.

### Display Metasploit Database Configuration

Displays the Metasploit database configuration file, showing database names, users, and connection parameters:

```bash
cat /usr/share/metasploit-framework/config/database.yml

```

**Notes:**

Example content (passwords are base64-encoded; decode with `echo <encoded> | base64 -d`):

```
development:
  adapter: postgresql
  database: msf
  username: msf
  password: OZVTON7Mp4p2nbVUMx4krV6+XAKSlbTvdEHD9nMiSEU=
  host: localhost
  port: 5432
  pool: 5
  timeout: 5

production:
  adapter: postgresql
  database: msf
  username: msf
  password: OZVTON7Mp4p2nbVUMx4krV6+XAKSlbTvdEHD9nMiSEU=
  host: localhost
  port: 5432
  pool: 5
  timeout: 5

test:
  adapter: postgresql
  database: msf_test
  username: msf
  password: OZVTON7Mp4p2nbVUMx4krV6+XAKSlbTvdEHD9nMiSEU=
  host: localhost
  port: 5432
  pool: 5
  timeout: 5

```

### Launch msfconsole

Launches msfconsole in normal mode, quiet mode, or displays CLI help:

```bash
msfconsole

```

```bash
msfconsole -q

```

```bash
msfconsole -h

```

**Notes:**

- Normal mode: Full interactive console.
- `q`: Quiet mode (suppresses banner and startup messages).
- `h`: Shows command-line help.

### General Help in msfconsole

Shows how to get general help and a list of commands from within msfconsole:

```
msf6 > ?
# To get help and command list

```

```
msf6 > help
# To get help and command list

```

**Notes:**

- Use `?` or `help` interchangeably for core command overview.
- Commands are case-insensitive.

### Help for Show Command

Displays help for the show command and lists different types of modules and components:

```
msf6 > show -h
# Help for 'show'

```

```
msf6 > show all
# List all components

```

```
msf6 > show encoders
# List all encoders

```

```
msf6 > show exploits
# List all exploits

```

```
msf6 > show payloads
# List all payloads

```

**Notes:**

- `show all`: Displays options, targets, payloads, encoders, nops, and description for the current module.
- Filter by type (e.g., `show exploits` lists all exploits).

## Search Command Help and Usage

Provides help for the search command and demonstrates different ways to search modules by keyword, name, type, author, or check support.

### Show Help for Search

Shows help for the search command using the long form help syntax inside msfconsole:

```
msf6 > help search
# Help on search

```

Shows help for the search command using its own -h (help) option:

```
msf6 > search -h
# Help on search

```

### Search Examples

Searches for all modules related to the SMB protocol (any module whose name, description, or references match smb):

```
msf6 > search smb

```

Searches for modules whose descriptive name field contains smb:

```
msf6 > search name:smb

```

Searches specifically for modules of type payload that are related to Meterpreter:

```
msf6 > search meterpreter type:payload

```

Searches for modules of type payload that are related to SMB:

```
msf6 > search smb type:payload

```

Searches for modules whose descriptive name contains sql (for example SQL-related exploits, auxiliary modules, etc.):

```
msf6 > search name:sql

```

Searches for modules written by the author dookie:

```
msf6 > search author:dookie

```

Searches for SMB-related modules that also have a check method available:

```
msf6 > search smb check:yes

```

**Notes:**

- Search is case-insensitive and supports operators like `type:`, `name:`, `author:`, `check:yes/no`.
- Use quotes for phrases: `search "exact phrase"`.
- Ranks results by relevance.

## Select and Use Modules

Selects an exploit module either by index or by its full path, and shows how to go back to the main prompt:

```
msf6 > search ms08_067_netapi

```

```
msf6 > use 5

```

```
msf6 > back

```

```
msf6 > use exploit/windows/smb/ms08_067_netapi

```

**Notes:**

- `use <index>` selects by search result number.
- `back` returns to the top-level prompt without unloading the module.

### Show Module Details

Displays configurable options and detailed information for the currently selected exploit module:

```
msf6 > show options

```

```
msf6 > show info

```

**Notes:**

- `show options`: Lists required/advanced options (e.g., RHOSTS, PAYLOAD).
- `show info`: Shows module description, references, targets, and metadata.

## Database Management

Works with the Metasploit database: getting help, checking status, and connecting manually.

### Database Help and Status

```bash
msf6 > help db

```

```
msf6 > db_status

```

```
msf6 > db_connect msf:password@127.0.0.1:5432/msf

```

**Notes:**

- `db_status`: Shows if connected (e.g., "connected to msf").
- `db_connect`: Manual connection (replace with actual credentials).

## Manage Workspaces

Manages workspaces to separate different engagements or projects:

```
msf6 > workspace -h

```

```
msf6 > workspace -a PT107

```

```
msf6 > workspace -a PT108

```

```
msf6 > workspace -d PT108

```

```
msf6 > workspace

```

```
msf6 > workspace PT107

```

**Notes:**

- `a`: Add new workspace.
- `d`: Delete workspace.
- `workspace`: List all.
- `workspace <name>`: Switch to specific.

## Use db_nmap for Scanning

Uses db_nmap to run Nmap scans directly from Metasploit and store the results in the database:

```
msf6 > db_nmap -sn 10.211.195.1/24

```

```
msf6 > db_nmap -v -sT -p- 192.168.1.31

```

```
msf6 > db_nmap -v -sT -sV -pA 192.168.1.212

```

**Notes:**

- Results auto-populate hosts/services in DB.
- Use standard Nmap flags (e.g., `sn` ping scan, `sT` TCP connect, `sV` version detection).

### Use Hosts from Database with db_nmap

Still need to pass their IPs as targets to db_nmap (Metasploit does not auto-use the hosts table as targets).

### Confirm Hosts in the DB

```
msf6 > hosts

```

### Export Hosts from Metasploit

```
msf6 > hosts -o /home/armour/Downloads/PT107-hosts.csv

```

### Build a List of IPs (in a Shell)

```bash
cat /home/armour/Downloads/PT107-hosts.csv | awk -F, '{print $1}' | tr -d ' ' | sort | grep -iv address > /home/armour/Downloads/hostlist.txt

```

### Scan All These Hosts with db_nmap

```
msf6 > db_nmap -v -Pn -sT -sV -A -sC -p- -iL /home/armour/Downloads/hostlist.txt

```

**Notes:**

- `iL`: Input file with IPs.
- `Pn`: Skip host discovery.
- `sC`: Default scripts.

## Manage Discovered Hosts

Manages discovered hosts in the database: listing, deleting, naming, annotating, and filtering:

```
msf6 > hosts

```

```
msf6 > hosts -h

```

```
msf6 > hosts -d 192.168.1.200

```

```
msf6 > hosts -n kali 192.168.1.6

```

```
msf6 > hosts -I "My kali" 192.168.1.6

```

```
msf6 > hosts -a "Dont Scan" 192.168.1.6

```

```
msf6 > hosts -c address

```

```
msf6 > hosts -c address,os_name

```

```
msf6 > hosts 192.168.1.200

```

```
msf6 > hosts -S Windows

```

**Notes:**

- `d`: Delete host.
- `n`: Name host.
- `I`: Add info/comment.
- `a`: Add tag.
- `c`: Select columns.
- `S`: Search (e.g., OS name).

### Additional Host Management Examples

```
msf6 > hosts -d 192.168.1.7

```

```
msf6 > hosts -n Router 192.168.1.1

```

```
msf6 > hosts -I "My Router" 192.168.1.1

```

```
msf6 > hosts -a 192.168.1.7

```

```
msf6 > hosts -m "Dont Scan" 192.168.1.7

```

```
msf6 > hosts -c address

```

```
msf6 > hosts -c address,os_name

```

```
msf6 > hosts -a 192.168.1.200

```

```
msf6 > hosts -S Windows

```

**Notes:**

- `m`: Add comment (alternative to `I`).
- Use tags for categorization (e.g., `hosts -a critical`).

## Metasploit Services Command

Views and filters service information discovered during scans.

### Basic Usage

Shows help and all available options for the command:

```
msf6 > services -h

```

Lists all known services (ports, states, names, etc.) from the database:

```
msf6 > services

```

### Filter by Port

Shows only services running on port 80 (typically HTTP):

```
msf6 > services -p 80

```

Shows only services on port 445 (often SMB):

```
msf6 > services -p 445

```

**Notes:**

- Filters by exact port number.

### Filter by Host

Shows all services associated with host 192.168.1.31:

```
msf6 > services 192.168.1.31

```

### Select Columns

Limits the output to just the port and state columns for each service:

```
msf6 > services -c port,state

```

**Notes:**

- Comma-separated columns (e.g., `port,host,state,name,info`).

### Scan with db_nmap (Related to Services)

Scan all these hosts with db_nmap:

```
msf6 > db_nmap -v -Pn -sT -sV -A -sC -p- -iL /home/armour/Downloads/hostlist.txt

```

**Notes:**

- Populates services table with version detection (`sV`).

## Working with Exploits, Data, Credentials, Loot, and Sessions

### Load an Exploit Module

Selects and loads the ms08_067_netapi exploit module for the MS08-067 Windows SMB vulnerability:

```
msf6 > use exploit/windows/smb/ms08_067_netapi

```

**Notes:**

- Sets the current module for configuration.

### View Vulnerabilities

Lists vulnerabilities stored in the database, typically populated from scans or exploitation results, showing host, port, and references like CVEs:

```
msf6 > vulns

```

**Notes:**

- Filters: `vulns -h` for options.

### Import Scan Data

Imports external scan results (for example, Nmap/Nessus XML) into the Metasploit database so hosts, services, and vulns appear under hosts, services, etc.:

```
msf6 > db_import /home/armour/vm/192.168.1.244/192.168.1.244_ver_sc.xml

```

**Notes:**

- Supports XML, Nmap, Nessus, etc. Verify with `db_status`.

### Export Database Data

Shows help and options for exporting database contents (formats, output path, etc.):

```
msf6 > db_export -h

```

Exports the current workspace database (hosts, services, vulns, creds, loot) to an XML file:

```
msf6 > db_export -f xml /home/armour/vm/msf_host.xml

```

**Notes:**

- `f`: Format (xml, csv, etc.).
- Includes all DB data.

### View Stored Credentials

Displays all credentials captured or imported (usernames, passwords, hashes, related services):

```
msf6 > creds

```

**Notes:**

- Filters: `creds -h`; e.g., `creds -t hash` for hashes only.

### View Loot

Lists loot files and data (config files, dumps, captured documents, etc.) stored from post-exploitation modules:

```
msf6 > loot

```

**Notes:**

- Download with `download /path/to/loot`.

### Manage Sessions

Shows help and usage options for session management (list, interact, kill, background, etc.):

```
msf6 > sessions -h

```

Lists all active sessions with IDs, types (meterpreter/shell), hosts, and users:

```
msf6 > sessions -l

```

Interacts with the session having ID 1, attaching your console to that shell or Meterpreter session:

```
msf6 > sessions -i 1

```

**Notes:**

- `l`: List sessions.
- `i <id>`: Interact (use `background` to return to console).
- `K`: Kill all sessions.
