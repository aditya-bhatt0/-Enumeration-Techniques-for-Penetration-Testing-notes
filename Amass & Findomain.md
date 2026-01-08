**Amass GitHub Repository:** https://github.com/OWASP/Amass

Amass is a powerful tool for network mapping and attack surface discovery. It performs subdomain enumeration, DNS reconnaissance, and data visualization to help map a target's external attack surface during security assessments.

Amass has several subcommands:

- **intel**: For discovering related domains and information about the target organization.
- **enum**: For performing subdomain enumeration and attack surface mapping.
- **viz**: For visualizing enumeration results (e.g., graphs).
- **track**: For tracking changes between multiple enumerations over time.
- **db**: For querying and manipulating the Amass graph database.

## **Installation**

### **Install Amass on Debian/Ubuntu-based systems**

```
apt install amass

```

### **Alternative: Install latest version via Go**

```
go install -v github.com/OWASP/Amass/v4/...@master

```

## **Basic Commands**

### **Check if Amass is installed correctly**

```
amass

```

### **Display general help information**

```
amass --help

```

## **Configuration Files**

Amass supports configuration files for custom data sources, API keys, resolvers, etc.

- **Example config file:** `/opt/amass/config.ini` or `~/.config/amass/config.ini`
- **Example datasources file:** `datasources.yaml`

## **Intel Subcommand**

The **intel** subcommand is used to gather information about the target organization (e.g., related domains, ASNs, WHOIS data).

### **Display help for intel subcommand**

```
amass intel --help

```

## **Enum Subcommand**

The **enum** subcommand performs subdomain enumeration.

### **List all available data sources**

```
amass enum --list

```

### **List available sources using a custom configuration file**

```
amass enum --list -config /opt/amass/config.ini

```

### **Perform passive enumeration (no DNS queries or brute-forcing)**

```
amass enum -passive -d tesla.com

```

### **Passive enumeration with verbose output (shows sources)**

```
amass enum -passive -d tesla.com -v

```

### **Passive enumeration with custom configuration file**

```
amass enum -passive -d tesla.com -v -config /opt/amass/config.ini

```

### **Active enumeration with brute-forcing using a small wordlist**

```
amass enum -active -brute -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -d tesla.com -o tesla_amass.txt

```

### **Active enumeration with larger wordlist and source/IP details**

```
amass enum -active -d tesla.com -brute -w /usr/share/seclists/Discovery/DNS/dns-Jhaddix.txt -src -ip -dir amass_tesla -o amass_results.txt

```

### **Active enumeration with custom resolvers and output**

```
amass enum -active -d tesla.com -brute -w /usr/share/seclists/Discovery/DNS/dns-Jhaddix.txt -rf /opt/massdns/lists/resolvers.txt -src -ip -dir amass_tesla -o amass_results.txt

```

# **Findomain**

Findomain is a fast and powerful subdomain enumeration tool designed for security professionals. It uses multiple data sources including Certificate Transparency logs, DNS datasets, and APIs for comprehensive passive/active discovery.

## **Installation**

### **Option 1: Install via Package Manager (if available)**

```
apt install findomain

```

### **Option 2: Download Latest Release Manually**

### **Download the latest Findomain release**

```
curl -LO <https://github.com/Findomain/Findomain/releases/latest/download/findomain-linux.zip>

```

### **Extract the archive**

```
unzip findomain-linux.zip

```

### **Make the binary executable**

```
chmod +x findomain

```

### **Move binary to system directory for global access**

```
cp -v findomain /usr/local/bin/

```

### **Strip debugging symbols to reduce binary size**

```
strip -s /usr/local/bin/findomain

```

### **Verify Installation**

### **Check installation and display help menu**

```
findomain --help

```

## **Running Findomain for Subdomain Enumeration**

### **Basic: Enumerate subdomains and save to file**

```
findomain -t tesla.com -u output.txt

```

### **Output results directly to console (no file)**

```
findomain -t tesla.com

```

### **Enumerate multiple domains from a file**

```
findomain -f domains.txt -u results.txt

```

### **Use with API key (for premium sources if configured)**

```
findomain -t tesla.com --api-key YOUR_API_KEY

```

**For more advanced options and configuration (API keys, resolvers, etc.), refer to the official documentation:**

https://github.com/Findomain/Findomain
