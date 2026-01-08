## **Default Ports / Common DNS Ports**

- **TCP:** 53
- **UDP:** 53

## **Example Commands**

### **Scan DNS Ports using nmap**

```
nmap -v -sV -sT -sU -p T:53,U:53 192.168.1.43

```

### **Comprehensive DNS service scan with nmap**

```
nmap -v -Pn -sV -sC -sT -sU -p T:53,U:53 192.168.1.43

```

### **Test DNS connectivity using netcat (TCP)**

```
nc -v 192.168.1.43 53

```

### **Test DNS connectivity using netcat (UDP)**

```
nc -vu 192.168.1.43 53

```

## **DNS Records**

| **Record Type** | **Description** |
| --- | --- |
| **A** | Address Mapping record. Maps a hostname to an IPv4 address. |
| **AAAA** | IPv6 Address record. Maps a hostname to an IPv6 address. |
| **CNAME** | Canonical Name record. Maps a domain to another domain name. |
| **NS** | Name Server record. Specifies the authoritative DNS server for the domain. |
| **PTR** | Reverse Lookup Pointer record. Resolves an IP address to a hostname. |
| **SOA** | Start of Authority record. Provides information about the DNS zone. |
| **HINFO** | Host Information record. Specifies details about the host's CPU and OS. |
| **MX** | Mail Exchange record. Specifies the mail servers for the domain. |
| **TXT** | Text record. Stores arbitrary text data, often used for SPF, DKIM, etc. |

## **Installing and Verifying Bind-Utils**

### **Check if bind-utils is installed**

```
rpm -qa | grep bind

```

### **Install bind-utils using yum**

```
yum install bind-utils

```

### **Check the installed version of bind-utils**

```
rpm -qi bind-utils

```

### **List the installed files from bind-utils package**

```
rpm -ql bind-utils

```

## **Common DNS Client Tools in bind-utils**

| **Tool** | **Path** | **Description** |
| --- | --- | --- |
| **dig** | /usr/bin/dig | Query DNS records |
| **host** | /usr/bin/host | Simple DNS lookup |
| **nslookup** | /usr/bin/nslookup | Interactive DNS query tool |

## **DNS Queries using dig**

### **Basic Usage**

### **Query a domain's DNS records**

```
dig google.com

```

### **Specify a custom DNS server**

```
dig google.com @64.6.64.6

```

### **Query Specific DNS Records**

### **A Record (IPv4)**

```
dig google.com A

```

### **AAAA Record (IPv6)**

```
dig google.com AAAA

```

### **NS Record (Name Server)**

```
dig google.com NS

```

### **MX Record (Mail Exchange)**

```
dig google.com MX

```

### **SOA Record (Start of Authority)**

```
dig google.com SOA

```

### **TXT Record (Text Information)**

```
dig google.com TXT

```

### **ALL Records**

```
dig google.com ANY

```

### **HINFO Record (Host Information)**

```
dig armourinfosec.com HINFO

```

### **Simplified Output**

### **Shortened output for cleaner results**

```
dig google.com +short

```

### **Shortened output for specific records**

```
dig armourinfosec.com NS +short

```

```
dig armourinfosec.com MX +short

```

```
dig google.com ANY +short

```

### **Customizing Output**

### **Suppress all output**

```
dig google.com +noall

```

### **Show only the answer section**

```
dig google.com +noall +answer

```

### **Show both question and answer sections**

```
dig google.com +noall +answer +question

```

### **Clean output by removing comments and unnecessary sections**

```
dig google.com +nocomments +noquestion +noauthority +noadditional +nostats

```

### **Batch Domain Queries**

### **Create a list of domains**

```
vim domain_list.txt

```

**Example content:**

```
google.com
facebook.com
armourinfosec.com
youtube.com

```

### **Query multiple domains from a file**

```
dig -f domain_list.txt

```

### **Short output for batch queries**

```
dig -f domain_list.txt +short

```

### **Batch Reverse DNS Lookup**

### **Create a list of IP addresses**

```
vim ip_add_list.txt

```

**Example content:**

```
8.8.8.8
64.6.64.6
8.8.4.4
20.20.20.20

```

### **Format the list using awk**

```
awk '{print "-x " $0}' ip_add_list.txt > ip_add_list2.txt

```

### **Format the list using sed**

```
cat ip_add_list.txt | sed -e 's/.*/-x &/' > ip_add_list3.txt

```

### **Perform batch reverse lookup**

```
dig -f ip_add_list2.txt +short

```

### **Alternative batch lookup (using -x directly on multiple IPs)**

```
dig -x 8.8.8.8 -x 64.6.64.6 -x 8.8.4.4 +short

```

## **DNS Queries using host**

The **host** command is a simple DNS lookup utility used to convert domain names to IP addresses and retrieve other DNS records.

### **Basic Usage**

### **Lookup IP address for a domain**

```
host google.com

```

### **Query Specific DNS Records**

### **Lookup NS (Name Server) records**

```
host -t ns google.com

```

### **Lookup MX (Mail Exchange) records**

```
host -t mx google.com

```

### **Lookup SOA (Start of Authority) record**

```
host -t soa google.com

```

## **DNS Queries using nslookup**

The **nslookup** (Name Server Lookup) command is used to query Internet name servers interactively or non-interactively for information about hostnames, IP addresses, and DNS records.

### **Basic Usage**

### **Lookup IP address for a domain**

```
nslookup google.com

```

### **Specify a custom DNS server**

```
nslookup google.com 64.6.64.6

```

### **Query Specific DNS Records**

### **Lookup A (IPv4) records**

```
nslookup -type=A google.com

```

### **Lookup AAAA (IPv6) records**

```
nslookup -type=AAAA google.com

```

### **Lookup NS (Name Server) records**

```
nslookup -type=NS google.com

```

### **Lookup MX (Mail Exchange) records**

```
nslookup -type=MX google.com

```

### **Lookup SOA (Start of Authority) record**

```
nslookup -type=SOA google.com

```

### **Lookup TXT (Text) records**

```
nslookup -type=TXT google.com

```

### **Lookup all available records**

```
nslookup -type=ANY google.com

```

### **Lookup Specific Domain**

### **Lookup specific domain (example: [armourinfosec.com](http://armourinfosec.com/))**

```
nslookup www.armourinfosec.com

```

### **Interactive Mode**

You can use **nslookup** in interactive mode to perform multiple queries:

```
nslookup

```

**Example interactive session:**

```
> facebook.com
> set type=NS
> facebook.com
> server 64.6.64.6
> facebook.com
> exit

```
