---

## What is DNS Enumeration?

DNS Enumeration is the process of gathering information about a domain and its DNS records to understand the domain’s infrastructure. This includes identifying subdomains, name servers, mail servers, IP addresses, and other publicly available details that may expose potential attack vectors.

DNS enumeration is commonly used during **information gathering** phases of penetration testing and red teaming.

---

## Domain Correlation

### 1. Vertical Domain Correlation

Vertical domain correlation involves identifying **subdomains related to a main (parent) domain**.

**Examples:**

**armourinfosec.com**

- admin.armourinfosec.com
- mail.armourinfosec.com

**tesla.com**

- admin.tesla.com
- mail.tesla.com
- zoom.tesla.com
- cp.tesla.com
- test.tesla.com

---

### 2. Horizontal Domain Correlation

Horizontal domain correlation involves identifying **related domains across different TLDs or organizations** that belong to the same entity.

**Examples:**

**armourinfosec-related domains**

- thehackersworld.com
- infosecwarrior.com
- armourinfosec.in

**tesla-related domains**

- .tesla.cn
- .teslamotors.com
- .tesla.services
- .teslainsuranceservices.com
- .solarcity.com

---

## Why DNS Enumeration is Important

1. **Information Gathering**
    
    Helps attackers and security professionals collect details about a target’s network and infrastructure.
    
2. **Identifying Attack Surface**
    
    Misconfigured DNS records, exposed subdomains, and outdated systems can act as entry points.
    
3. **Social Engineering**
    
    Collected DNS data can be used to craft realistic phishing or social engineering attacks.
    
4. **Penetration Testing & Red Teaming**
    
    Understanding DNS infrastructure allows testers to simulate real-world attack scenarios effectively.
    

---

## Types of DNS Enumeration

1. **Direct Domain Enumeration**
    
    Identifying subdomains and DNS records through direct DNS queries.
    
2. **Reverse DNS Lookup**
    
    Resolving an IP address back to its corresponding domain name.
    
3. **Zone Transfer (AXFR) Enumeration**
    
    Attempting to retrieve the entire DNS zone file from a misconfigured DNS server.
    
4. **Brute-Force Subdomain Discovery**
    
    Using wordlists to test common subdomain names and identify valid ones.
    
5. **PTR Record Lookup**
    
    Mapping IP addresses to domain names using PTR records.
    
6. **Certificate Transparency Logs**
    
    Discovering subdomains listed in publicly available SSL/TLS certificate logs.
    
7. **WHOIS Lookup**
    
    Gathering domain registration details such as owner, registrar, and contact information.
    

---
