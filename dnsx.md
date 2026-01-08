dnsx is a fast and multi-purpose DNS toolkit from ProjectDiscovery that enables bulk DNS enumeration using custom resolvers. It is widely used in reconnaissance, asset discovery, and bug bounty workflows.

## **Installing dnsx**

### **Using go install (Recommended - Requires Go 1.21+)**

```
go install -v github.com/projectdiscovery/dnsx/cmd/dnsx@latest

```

### **Using go get (For Older Go Versions)**

```
GO111MODULE=on go get -v github.com/projectdiscovery/dnsx/cmd/dnsx

```

### **Moving the Binary to a System Path**

```
cp -v ~/go/bin/dnsx /usr/local/bin/

```

## **Verifying Installation**

### **Check Binary Execution**

```
dnsx

```

### **Display Installed Version**

```
dnsx -version

```

### **Show Help Menu and Options**

```
dnsx -h

```

## **Running dnsx Queries**

## **Reverse DNS Lookup (PTR Records)**

PTR lookups resolve IP addresses back to domain names. This is useful for discovering hostnames and infrastructure mapping.

### **Single IP Reverse Lookup**

```
echo "8.8.8.8" | dnsx -silent -resp-only -ptr

```

### **Bulk Reverse Lookup from a File**

```
vim ip-list.txt

```

**Example content:**

```
8.8.8.8
8.8.4.4
64.6.64.6

```

```
dnsx -l ip-list.txt -silent -resp-only -ptr

```

## **Scanning CIDR Ranges**

Performs reverse DNS enumeration over an entire IP range using PTR records.

### **Scan /16 range**

```
echo "8.8.0.0/16" | dnsx -silent -resp-only -ptr

```

### **Scan /23 range**

```
echo "106.201.194.0/23" | dnsx -silent -resp-only -ptr

```

### **Scan /28 range**

```
echo "82.196.42.196/28" | dnsx -silent -resp-only -ptr

```

### **Scan /24 range**

```
echo "82.196.42.0/24" | dnsx -silent -resp-only -ptr

```

## **Filtering Dead Records**

Filters out non-resolving or inactive domains from a list (shows only resolving hosts).

### **Show only resolving hosts**

```
cat tesla_domain.txt | dnsx

```

### **Show resolving hosts with full response**

```
cat tesla_domain.txt | dnsx -resp

```

## **Extracting A Records**

Resolves domains to IPv4 addresses. Commonly used to identify live hosts.

### **Extract A records (with hostname)**

```
cat tesla_domain.txt | dnsx -silent -a -resp

```

### **Extract A records from file**

```
dnsx -l tesla-all_domains.txt -silent -a -resp

```

### **Extract only IP addresses**

```
cat tesla_domain.txt | dnsx -silent -a -resp-only

```

### **Extract only IP addresses from file**

```
dnsx -l tesla-all_domains.txt -silent -a -resp-only

```

## **Querying Multiple Record Types**

Performs multiple DNS record queries in a single run to gather comprehensive DNS data.

### **Query A, AAAA, NS, PTR, CNAME**

```
dnsx -l airbnb.com.txt -silent -a -aaaa -ns -ptr -cname -resp

```

### **Query A, AAAA, NS, PTR, CNAME (alternative example)**

```
dnsx -l tesla-all_domains.txt -silent -a -aaaa -ns -ptr -cname -resp

```

## **Extracting CNAME Records**

Identifies canonical name mappings, often useful for detecting third-party services and subdomain takeovers.

### **Extract CNAME with hostname**

```
dnsx -l tesla-all_domains.txt -silent -cname -resp

```

### **Pipe CNAME extraction**

```
cat tesla_domain.txt | dnsx -silent -cname -resp

```

### **Extract only CNAME values**

```
cat tesla_domain.txt | dnsx -silent -cname -resp-only

```

## **Filtering by DNS Response Codes**

Limits output to specific DNS response codes (e.g., successful or error responses).

### **Filter CNAME by specific response codes**

```
cat tesla_domain.txt | dnsx -silent -cname -rcode noerror,servfail,refused

```

### **Filter CNAME from file by response codes**

```
dnsx -l tesla-all_domains.txt -silent -cname -rcode noerror,servfail,refused

```

### **Filter CNAME with custom resolvers**

```
dnsx -l tesla-all_domains.txt -silent -cname -rcode noerror,servfail,refused -r /opt/resolvers.txt

```

## **Querying NS, MX, and TXT Records**

Used to gather DNS configuration details such as nameservers, mail servers, and SPF/DKIM records.

### **NS Records**

```
dnsx -l tesla_domain.txt -ns -resp

```

### **MX Records**

```
dnsx -l tesla_domain.txt -mx -resp

```

### **TXT Records**

```
dnsx -l tesla_domain.txt -txt -resp

```

## **Extracting Subdomains from Network Ranges**

Uses PTR records to identify hostnames associated with an IP range.

### **Extract hostnames via PTR**

```
echo 8.21.0.0/16 | dnsx -silent -resp-only -ptr

```

## **Using Custom Resolvers**

Custom resolvers improve accuracy, reduce rate limiting, and avoid poisoned DNS responses.

### **Download Reliable Resolver List**

```
wget <https://raw.githubusercontent.com/blechschmidt/massdns/master/lists/resolvers.txt> -O /opt/resolvers.txt

```

## **Saving Output**

Stores resolved DNS data for later analysis or reporting.

### **Save CNAME records with custom resolvers**

```
dnsx -l tesla-all_domains.txt -silent -cname -r /opt/resolvers.txt -o tesla-cname.txt

```

### **Save only CNAME values with custom resolvers**

```
dnsx -l tesla-all_domains.txt -silent -cname -resp-only -r /opt/resolvers.txt -o tesla-cname.txt

```

### **Output in JSON Format**

```
dnsx -l tesla-all_domains.txt -silent -cname -resp-only -r /opt/resolvers.txt -json -o tesla-cname.json

```

### **Parsing JSON Output Using jq**

```
jq . tesla-cname.json

```

**References**

Official dnsx documentation and source code:

https://github.com/projectdiscovery/dnsx
