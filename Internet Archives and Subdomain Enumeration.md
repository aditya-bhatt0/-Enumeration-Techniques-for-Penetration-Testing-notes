Internet Archives are large-scale web crawlers and indexing systems that periodically scan websites and store historical snapshots of their content. These archives are extremely valuable for subdomain enumeration, as they often reveal infrastructure that no longer appears in DNS or search engines.

By analyzing historical records, we can:

- Discover old, forgotten, or deprecated subdomains
- Generate permutations to uncover additional valid subdomains
- Improve passive reconnaissance during penetration testing and bug bounty hunting

## **Extracting Historical Subdomains**

### **Querying Internet Archives for URLs**

We use tools like **waybackurls**, **gau**, or **gauplus** to retrieve archived URLs for a target domain.

## **waybackurls**

**GitHub Repository:** https://github.com/tomnomnom/waybackurls

waybackurls is a tool developed by tomnomnom that extracts URLs from the Wayback Machine (Internet Archive) for a given domain. This is useful for reconnaissance, subdomain enumeration, and discovering old endpoints.

### **Installation**

```
go install github.com/tomnomnom/waybackurls@latest

```

### **Fetching URLs from Wayback Machine**

```
waybackurls tesla.com

```

```
waybackurls tesla.com > tesla.com-waybackurls.txt

```

### **Extracting Only Subdomains**

```
waybackurls tesla.com | unfurl -u domains

```

### **Saving the Output to a File**

```
waybackurls tesla.com | unfurl -u domains > waybackurls-output.txt

```

**Example Output:**

```
www.tesla.com
shop.tesla.com
blog.tesla.com
assets.tesla.com

```

## **gau (GetAllUrls)**

gau pulls URLs from multiple sources, including:

- Wayback Machine
- Common Crawl
- URLScan
- AlienVault's Open Threat Exchange (OTX)

### **Basic Usage**

```
gau tesla.com > tesla.com-gau.txt

```

## **gauplus**

**GitHub Repository:** https://github.com/bp0lr/gauplus

gauplus is an enhanced version of gau (GetAllUrls). It aggregates archived URLs from multiple public sources, making it extremely useful for reconnaissance, endpoint discovery, and subdomain enumeration.

### **Data Sources Used by gauplus**

- Wayback Machine — Historical snapshots of websites
- Common Crawl — Large-scale web crawl datasets
- AlienVault's OTX — URLs from threat intelligence
- [URLScan.io](http://urlscan.io/) — URLs captured during website scans
- Other public archives — Additional passive data sources

### **Installation**

```
go install github.com/bp0lr/gauplus@latest

```

### **Verify Installation**

```
gauplus -h

```

### **Fetch URLs for a Domain (including subdomains)**

```
gauplus -t 5 --random-agent --subs tesla.com

```

### **Save the output to a file**

```
gauplus -t 5 --random-agent --subs tesla.com > tesla.com-gauplus.txt

```

**Explanation of Common Flags:**

- `t 5` — Number of concurrent threads (default: 5 for speed)
- `-random-agent` — Uses random User-Agent strings to reduce blocking
- `-subs` — Includes all discovered subdomains in the results

### **Extracting Only Subdomains**

### **Extract domains to stdout**

```
cat tesla.com-gauplus.txt | unfurl -u domains

```

### **Save extracted domains to a file**

```
cat tesla.com-gauplus.txt | unfurl -u domains > tesla.com-domains.txt

```

### **One-liner (recommended)**

```
gauplus -t 5 --random-agent --subs tesla.com | unfurl -u domains > tesla.com-domains.txt

```

### **Pipe to remove duplicates**

```
gauplus --subs tesla.com | unfurl -u domains | sort -u

```

**Example Output:**

```
www.tesla.com
shop.tesla.com
blog.tesla.com
api.tesla.com
energy.tesla.com

```

**Tips & Best Practices:**

- Combine with tools like httpx, nuclei, or ffuf for further testing
- Best suited for passive reconnaissance (no direct interaction with target)

## **Extracting Unique Subdomains from URLs**

Archived tools return full URLs, but we usually only need the subdomains.

### **Using unfurl**

```
cat tesla.com-gau.txt | unfurl -u domains | sort -u > subdomains.txt

```

**Explanation:**

- `unfurl -u domains` — Extracts domain names from URLs
- `sort -u` — Removes duplicate entries

**Example Output (subdomains.txt):**

```
blog.tesla.com
mail.tesla.com
dev.tesla.com
api.tesla.com

```

## **dnsgen**

**GitHub Repository:** https://github.com/AlephNullSK/dnsgen (formerly ProjectAnte/dnsgen)

dnsgen is a permutation-based subdomain generation tool widely used in reconnaissance and bug bounty hunting.

**What dnsgen Does:**

- Takes a base domain or list of known subdomains
- Optional wordlists
- Generates new subdomain permutations (e.g., development, staging, API variants)
- Output is not validated — always resolve results using a DNS resolver like dnsx or massdns

### **Installation**

```
pip install dnsgen

```

### **Or from source**

```
git clone <https://github.com/AlephNullSK/dnsgen.git>

```

```
cd dnsgen

```

```
python3 setup.py install

```

### **Basic Usage**

### **Generate subdomains from a single domain**

```
dnsgen tesla.com

```

### **Generate subdomains using a wordlist**

```
dnsgen -w deepmagic.com-prefixes-top500.txt tesla.com

```

**Explanation:**

- `w` — Wordlist for prefixes/suffixes
- Input can be a file or piped list

## **Automating the Full Workflow**

Chain tools together for a streamlined passive enumeration process:

```
echo "tesla.com" | waybackurls | unfurl -u domains | sort -u | tee subdomains.txt

```

```
cat subdomains.txt | dnsgen - | tee permutations.txt

```

```
cat permutations.txt | dnsx -resp | tee valid_subdomains.txt

```

```
cat valid_subdomains.txt | httprobe | tee live_subdomains.txt

```

**This pipeline covers:**

- Historical URL extraction
- Subdomain extraction
- Permutation generation
- DNS validation
- Live service discovery

**Conclusion**

Internet archives combined with tools like waybackurls, gau/gauplus, unfurl, and dnsgen provide powerful passive methods for discovering hidden subdomains and expanding the attack surface during security assessments.
