## **Reconnaissance Overview**

Reconnaissance (Recon) is the process of gathering information about a target before attempting exploitation or penetration testing. It is considered the first phase of the attack lifecycle and plays a major role in identifying potential attack surfaces.

### **Types of Reconnaissance**

#### **Passive Reconnaissance**

Passive reconnaissance involves collecting information without directly interacting with the target systems.

Examples include:

* Looking up DNS records using public DNS servers  
* Reading company news articles  
* Searching employee information on social media  
* Reviewing job postings related to the target

Passive recon is generally safer and less likely to trigger alerts.

#### **Active Reconnaissance**

Active reconnaissance involves directly interacting with the target.

Examples include:

* Pinging servers  
* Connecting to services such as HTTP, FTP, or SMTP  
* Social engineering attempts  
* Physical access attempts

Active recon is more intrusive and may create logs or alerts.

### **Key Takeaways**

* Passive recon relies on publicly available information.  
* Active recon directly engages with the target environment.  
* Active reconnaissance can lead to legal issues if performed without authorization.

### **Question and Answer**

**You visit the Facebook page of the target company, hoping to get some of their employee names. What kind of reconnaissance activity is this? (A for active, P for passive)**  
 P

**You ping the IP address of the company webserver to check if ICMP traffic is blocked. What kind of reconnaissance activity is this? (A for active, P for passive)**  
 A

**You happen to meet the IT administrator of the target company at a party. You try to use social engineering to get more information about their systems and network infrastructure. What kind of reconnaissance activity is this? (A for active, P for passive)**  
 A

## **WHOIS**

WHOIS is a protocol used to retrieve registration information about domain names. WHOIS servers listen on TCP port 43 and provide details about domain ownership and registration.

### **Information Gathered from WHOIS**

* Domain registrar  
* Registrant information  
* Domain creation date  
* Last update date  
* Expiration date  
* Name servers  
* Administrative and technical contacts

### **Example Command**

whois tryhackme.com

### **Important Findings from WHOIS**

WHOIS data can reveal:

* Infrastructure details  
* Potential targets for social engineering  
* Email addresses  
* DNS server information

### **Key Takeaways**

* WHOIS is useful for gathering domain registration information.  
* Privacy services may hide registrant details.  
* WHOIS data can help identify additional attack surfaces.

### **Question and Answer**

**When was TryHackMe.com registered?**  
 20180705

**What is the registrar of TryHackMe.com?**  
 namecheap.com

**Which company is TryHackMe.com using for name servers?**  
 cloudflare.com

## **DNS Enumeration with nslookup and dig**

DNS enumeration helps identify important information related to a target domain.

### **Common DNS Record Types**

| Record Type | Purpose |
| ----- | ----- |
| A | IPv4 Address |
| AAAA | IPv6 Address |
| MX | Mail Servers |
| TXT | Text Records |
| CNAME | Canonical Name |
| SOA | Start of Authority |

### **Using nslookup**

#### **Lookup IPv4 Addresses**

nslookup \-type=A tryhackme.com 1.1.1.1

#### **Lookup Mail Servers**

nslookup \-type=MX tryhackme.com

### **Using dig**

#### **Query MX Records**

dig tryhackme.com MX

#### **Query Using Specific DNS Server**

dig @1.1.1.1 tryhackme.com MX

### **Why DNS Enumeration Matters**

DNS records may reveal:

* Mail infrastructure  
* Cloud providers  
* Internal services  
* Third-party integrations  
* Additional IP addresses

### **Key Takeaways**

* nslookup is simple and easy to use.  
* dig provides more detailed DNS information.  
* TXT records may contain sensitive or hidden information.

### **Question and Answer**

**Check the TXT records of thmlabs.com. What is the flag there?**  
 THM{a5b83929888ed36acb0272971e438d78}

## **Subdomain Enumeration**

Standard DNS lookup tools such as nslookup and dig cannot automatically discover subdomains. Subdomains often reveal additional services and infrastructure that may not be publicly advertised.

Examples of possible subdomains:

* wiki.tryhackme.com  
* webmail.tryhackme.com  
* remote.tryhackme.com

### **Why Subdomains Matter**

Subdomains may expose:

* Internal portals  
* Development servers  
* Legacy applications  
* Remote access systems  
* Vulnerable or outdated services

### **Methods of Discovering Subdomains**

#### **Search Engines**

Using multiple search engines can reveal indexed subdomains.

Limitations:

* Time-consuming  
* Requires reviewing many results  
* Some subdomains are intentionally hidden

#### **Brute Force DNS Queries**

Automated tools can test large lists of possible subdomains against DNS records.

#### **DNSDumpster**

[DNSDumpster](https://dnsdumpster.com/?utm_source=chatgpt.com) is an online reconnaissance service that collects DNS information and displays it in organized tables and graphs.

### **Information Gathered by DNSDumpster**

* DNS servers  
* MX records  
* TXT records  
* IP addresses  
* Geolocation data  
* Related subdomains  
* Network relationships

### **Key Takeaways**

* Subdomains can reveal hidden infrastructure.  
* DNSDumpster automates DNS reconnaissance.  
* Graphical visualization helps map relationships between services.

### **Question and Answer**

**Lookup tryhackme.com on DNSDumpster. What is one interesting subdomain that you would discover in addition to www and blog?**  
 remote

## **Shodan.io**

[Shodan.io](https://www.shodan.io/?utm_source=chatgpt.com) is a search engine designed to index internet-connected devices and services.

Unlike traditional search engines that index websites, Shodan indexes:

* Servers  
* Routers  
* Cameras  
* IoT devices  
* Databases  
* Industrial systems

### **Information Collected by Shodan**

* IP addresses  
* Open ports  
* Service banners  
* Hosting providers  
* Geographic locations  
* Software versions

### **Uses in Reconnaissance**

#### **Offensive Security**

* Identify exposed services  
* Discover vulnerable systems  
* Gather server information passively

#### **Defensive Security**

* Monitor exposed assets  
* Detect accidental public exposure  
* Audit internet-facing infrastructure

### **Example Information from Shodan**

Searching for a domain or IP may reveal:

* Web server software  
* Apache/Nginx versions  
* SSL information  
* Open services and ports

### **Key Takeaways**

* Shodan is a passive reconnaissance tool.  
* It indexes publicly accessible internet-connected systems.  
* Useful for both attackers and defenders.

### **Question and Answer**

**According to Shodan.io, what is the first country in the world in terms of the number of publicly accessible Apache servers?**  
 United States

**Based on Shodan.io, what is the 3rd most common port used for Apache?**  
 8080

**Based on Shodan.io, what is the most common port used for nginx?**  
 80

## **Passive Reconnaissance Summary**

This room focused on passive reconnaissance techniques and tools used to gather information without directly interacting with the target.

### **Tools Covered**

| Purpose | Command Example |
| ----- | ----- |
| Lookup WHOIS record | whois tryhackme.com |
| Lookup DNS A records | nslookup \-type=A tryhackme.com |
| Lookup DNS MX records | nslookup \-type=MX tryhackme.com 1.1.1.1 |
| Lookup DNS TXT records | nslookup \-type=TXT tryhackme.com |
| Lookup DNS A records with dig | dig tryhackme.com A |
| Lookup DNS MX records with dig | dig @1.1.1.1 tryhackme.com MX |
| Lookup DNS TXT records with dig | dig tryhackme.com TXT |

### **Final Key Takeaways**

* Passive reconnaissance avoids direct interaction with the target.  
* DNS records can reveal infrastructure details.  
* WHOIS provides registration and ownership information.  
* DNSDumpster helps identify hidden subdomains.  
* Shodan indexes exposed internet-connected devices.  
* Passive recon can uncover valuable attack surfaces before active testing begins.

