Passive subdomain enumeration involves gathering subdomain information without directly querying the target's infrastructure. This reduces the chance of detection.

## **WHOIS Lookup**

Used to gather information about the registrant, name servers, and contact details.

### **Command: Perform WHOIS lookup on a domain**

```
whois armourinfosec.com

```

### **Web Tools for WHOIS**

- [who.is](http://who.is/)
- [godaddy.com/whois](http://godaddy.com/whois)
- [whoxy.com/reverse-whois](http://whoxy.com/reverse-whois)

## **Reverse WHOIS**

Find all domains registered with the same registrant or contact details.

### **Web Tools for Reverse WHOIS**

- [viewdns.info/reverse-whois/](http://viewdns.info/reverse-whois/)
- [reversewhois.io](http://reversewhois.io/)
- [whoxy.com/reverse-whois/](http://whoxy.com/reverse-whois/)

## **Google Analytics Relationship**

Use Google Analytics IDs to find related domains and potentially subdomains.

### **Example Web Tool**

```
<https://builtwith.com/relationships/tesla.com>

```

## **Certificate Transparency (CT) Logs**

Collect subdomains from publicly logged SSL/TLS certificates.

### **Web Tools for CT Logs**

- [crt.sh](http://crt.sh/)
- [censys.io](http://censys.io/)
- [developers.facebook.com/tools/ct](http://developers.facebook.com/tools/ct) (Facebook Certificate Transparency)
- [transparencyreport.google.com/https/certificates](http://transparencyreport.google.com/https/certificates) (Google Transparency Report)
- [sslmate.com/certspotter/](http://sslmate.com/certspotter/)

### **Example: Query [crt.sh](http://crt.sh/) using curl and process with jq**

```
curl -s "<https://crt.sh/?q=%.tesla.com&output=json>" | jq -r '.[].name_value' | sed 's/^*.//g' | sort -u

```

### **Example: Download [crt.sh](http://crt.sh/) data and process with jq and unfurl**

```
curl '<https://crt.sh/?q=%.tesla.com&output=json>' -o tesla.com.json

```

```
cat tesla.com.json | jq -r '.[].name_value' | unfurl -u domains > tesla.txt

```

## **Extract Subdomains from SAN (Subject Alternative Names)**

Extract SAN data from certificates to discover subdomains.

### **Example using Nmap**

```
nmap --script targets-asn --script-args targets-asn.asn=394161

```

### **Python Script Example**

```
python san_subdomain_enum.py google.com

```

## **VirusTotal**

Search for subdomain relationships in VirusTotal.

### **Example Web Tool**

```
<https://www.virustotal.com/gui/domain/www.tesla.com/relations>

```

## **Google Dorks**

Find subdomains using advanced Google search operators.

### **Examples of Google Dorks**

```
site:tesla.*

```

```
site:*.tesla.*

```

```
site:tesla.com -www -shop -xapps -autodiscover

```

## **Shodan and Censys**

Find exposed services and potentially associated subdomains.

### **Web Tools**

- [censys.io](http://censys.io/)
- [shodan.io](http://shodan.io/)
- [dnsdumpster.com](http://dnsdumpster.com/)

## **Rapid7 Sonar Datasets**

Use Rapid7's Forward DNS (fdns) datasets to find subdomains.

### **Example: Download and extract subdomains**

```
wget <https://opendata.rapid7.com/sonar.fdns_v2/2021-05-28-1622160364-fdns_a.json.gz>

```

```
pv 2021-05-28-1622160364-fdns_a.json.gz | pigz -dc | grep -E "\\.tesla\\.com\\"," | jq -r ".name"

```

## **Chaos Project Discovery**

Discover subdomains using Project Discovery's Chaos public dataset.

### **Example Script: Download and process Chaos dataset**

```
#!/bin/bash
Outputdir=/tmp/d1
mkdir -p $Outputdir/zipfils
mkdir -p $Outputdir/txtfils
wget "<https://chaos-data.projectdiscovery.io/index.json>" -O $Outputdir/index.json
grep "URL" $Outputdir/index.json | sed 's/.*"URL": "\\(.*\\)".*/\\1/' > $Outputdir/alltargets-zip.txt
wget -P $Outputdir/zipfils -i $Outputdir/alltargets-zip.txt
unzip -d $Outputdir/txtfils $Outputdir/zipfils/*.zip
cat $Outputdir/txtfils/*.txt > $Outputdir/alltargets.txt
rm -rf $Outputdir/zipfils $Outputdir/txtfils

```

## **GitHub Subdomain Enumeration**

Use GitHub search or tools to extract subdomains leaked in repositories.

### **Example using github-subdomains tool**

```
github-subdomains -d tesla.com -t <TOKEN>

```

## **Subdomain Enumeration Tools**

Common passive-capable or hybrid tools for subdomain enumeration:

- Sublist3r
- Amass (passive mode with -passive flag)
- Assetfinder
- Findomain
- Altdns
- Gobuster (with DNS mode)
