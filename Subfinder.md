Subfinder is a subdomain discovery tool that discovers valid subdomains for websites. It is designed as a passive framework, useful for bug bounties and safe for penetration testing.

## **Installation**

### **Install Subfinder using Go**

```
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest

```

### **Move binary to /usr/local/bin/ for global access**

```
cp -v ~/go/bin/subfinder /usr/local/bin/

```

### **Verify the installation**

```
subfinder -v

```

### **List all available sources**

```
subfinder -ls

```

**For more details, visit:**

https://github.com/projectdiscovery/subfinder

## **Post-Installation: Configuration**

To use API-based sources, configure API keys.

### **Create provider configuration file**

```
vim $HOME/.config/subfinder/provider-config.yaml

```

### **Example provider-config.yaml content**

```
binaryedge:
  - 0bf8919b-aab9-42e4-9574-d3b639324597
  - ac244e2f-b635-4581-878a-33f4e79a2c13
censys:
  - ac244e2f-b635-4581-878a-33f4e79a2c13:dd510d6e-1b6e-4655-83f6-f347b363def9
certspotter: []
passivetotal:
  - sample-email@user.com:sample_password
securitytrails: []
shodan:
  - AAAACLP1bJJSRMEYJazgwhJKrggRwKA
github:
  - ghp_lkyJGU3jv1xmwk4SDXavrLDJ4dl2pSJMzj4X
  - ghp_gkUuhkIYdQPj13ifH4KA3cXRn8JD2lqir2d4
zoomeye:
  - zoomeye_username:zoomeye_password

```

## **Usage Examples**

### **Basic Usage**

### **Run a simple scan for subdomains**

```
subfinder -d tesla.com

```

### **Verbose output**

```
subfinder -d tesla.com -v

```

### **Save output to a file**

```
subfinder -d tesla.com -o tesla_domain.txt

```

### **Count the number of discovered subdomains**

```
cat tesla_domain.txt | wc -l

```

### **Using Specific Sources**

### **Search using only [crt.sh](http://crt.sh/)**

```
subfinder -d tesla.com -s crtsh

```

### **Search using only Chaos dataset**

```
subfinder -d armourinfosec.com -s chaos

```

### **Search using multiple specific sources ([crt.sh](http://crt.sh/) and GitHub)**

```
subfinder -d tesla.com -s crtsh,github

```

```
subfinder -d tesla.com -s crtsh,github,chaos

```

### **Comprehensive Scan**

### **Perform a full scan using all available sources**

```
subfinder -d tesla.com -all

```

### **Full scan without wildcard filtering**

```
subfinder -d tesla.com -all -nw

```

### **Use a custom resolver list**

```
subfinder -d tesla.com -all -rL /opt/massdns/lists/resolvers.txt

```

### **Silent mode with custom resolvers and output file**

```
subfinder -d tesla.com -all -nw -silent -rL /opt/massdns/lists/resolvers.txt -o tesla_domain2.txt

```

### **Scanning Multiple Domains**

### **Create a file with multiple domains**

```
vim target_domain.txt

```

**Example content of target_domain.txt:**

```
google.com
google.co.in
google.us

```

### **Scan all domains from file and save output**

```
subfinder -dL target_domain.txt -o google_domain.txt

```

### **Count discovered subdomains**

```
cat google_domain.txt | wc -l

```

### **Silent Mode**

### **Scan a single domain in silent mode**

```
subfinder -silent -d youtube.com

```

### **Full scan for Facebook with verbose output and save results**

```
subfinder -all -d facebook.com -v -o subdomain-facebook.com.txt

```

### **Using echo to pipe domain input**

```
echo youtube.com | subfinder -silent

```

### **Using echo and save output**

```
echo youtube.com | subfinder -silent -o youtube_domain.txt

```

### **Full scan with custom resolver list (corrected path)**

```
subfinder -d tesla.com -all -r /opt/massdns/lists/resolvers.txt

```

## **Conclusion**

Subfinder is a powerful tool for passive subdomain enumeration. By using different sources and configurations, it can help security researchers efficiently map out a target's subdomains. Integrate it with other tools like Amass, Assetfinder, Findomain, and others for comprehensive reconnaissance.
