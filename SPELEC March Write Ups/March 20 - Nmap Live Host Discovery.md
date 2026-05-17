# **Network Segments and Subnetworks**

## **Overview**

A network segment is a group of computers connected through a shared communication medium such as:

* Ethernet switch  
* Wi-Fi access point

A subnetwork (subnet) is a logical division of an IP network. Each subnet has:

* Its own IP address range  
* A subnet mask  
* A router connection to larger networks

## **Key Difference**

| Term | Meaning |
| ----- | ----- |
| Network Segment | Physical connection |
| Subnetwork (Subnet) | Logical IP grouping |

## **Common Subnet Types**

### **/16 Subnet**

255.255.0.0

* Supports approximately 65,000 hosts

### **/24 Subnet**

255.255.255.0

* Supports approximately 250 hosts

# **ARP (Address Resolution Protocol)**

## **Overview**

ARP is used to discover the MAC address of devices on the same local subnet.

It works only within the local network segment because:

* ARP operates at Layer 2 (Link Layer)  
* Routers do not forward ARP packets

## **Important Concepts**

* ARP Request → Broadcast message  
* ARP Reply → Direct response from target device  
* Used before communication on Ethernet/Wi-Fi networks

## **ARP Limitations**

ARP cannot:

* Cross routers  
* Discover hosts outside the local subnet

## **ARP Scan Purpose**

ARP scanning is useful for:

* Host discovery  
* Internal network enumeration  
* Post-exploitation reconnaissance

## **Question and Answers**

### **How many devices can see the ARP Request from computer1?**

Answer:

4

### **Did computer6 receive the ARP Request?**

Answer:

nay

### **How many devices can see the ARP Request from computer4?**

Answer:

4

### **Did computer6 reply to the ARP Request?**

Answer:

yea

# **Nmap Target Specification**

## **Overview**

Nmap allows different target formats for scanning networks and systems.

## **Target Types**

### **List**

nmap MACHINE\_IP scanme.nmap.org example.com

Scans:

* 3 separate targets

### **Range**

10.11.12.15-20

Scans:

* 10.11.12.15 through 10.11.12.20

### **Subnet**

MACHINE\_IP/30

Scans:

* 4 IP addresses

### **Input File**

nmap \-iL list\_of\_hosts.txt

## **Listing Targets Without Scanning**

nmap \-sL TARGETS

### **Disable DNS Resolution**

nmap \-n \-sL TARGETS

## **Question and Answers**

### **What is the first IP address Nmap would scan if provided 10.10.12.13/29?**

Answer:

10.10.12.8

### **How many IP addresses will Nmap scan in range 10.10.0-255.101-125?**

Answer:

6400

# **Nmap Host Discovery**

## **Overview**

Host discovery determines which systems are online before performing port scans.

## **Default Nmap Discovery Methods**

### **Local Network (Privileged User)**

Uses:

* ARP Requests

### **External Network (Privileged User)**

Uses:

* ICMP Echo Requests  
* TCP ACK to port 80  
* TCP SYN to port 443  
* ICMP Timestamp Requests

### **External Network (Unprivileged User)**

Uses:

* TCP SYN packets to ports 80 and 443

## **Disable Port Scanning**

nmap \-sn TARGETS

# **ARP Scan with Nmap**

## **Command**

nmap \-PR \-sn TARGETS

## **Purpose**

* Discover live hosts using ARP only  
* Effective on local subnet  
* Fast and reliable

## **Example**

nmap \-PR \-sn 10.200.6.0/24

## **Key Characteristics**

* Sends ARP requests sequentially  
* Broadcast MAC address used  
* Live hosts respond with MAC addresses

## **Question and Answer**

### **How many hosts were found alive after scanning CONNECTION\_IP/24?**

Answer:

1

# **ICMP Echo Scan**

## **Overview**

ICMP Echo Requests are traditional “ping” packets used to determine whether a host is online.

## **Nmap Option**

\-PE

## **Example**

nmap \-PE \-sn 10.200.6.0/24

## **Important Notes**

* Uses ICMP Type 8 (Echo Request)  
* Expects ICMP Type 0 (Echo Reply)  
* Firewalls may block responses  
* Windows Firewall blocks ICMP Echo by default

# **ICMP Timestamp Scan**

## **Overview**

Uses ICMP Timestamp Requests to identify active hosts.

## **Nmap Option**

\-PP

## **ICMP Types**

| Request | Reply |
| ----- | ----- |
| Type 13 | Type 14 |

## **Example**

nmap \-PP \-sn 10.200.6.0/24

## **Characteristics**

* Alternative when ICMP Echo is blocked  
* Useful for bypassing filtering rules

# **ICMP Address Mask Scan**

## **Overview**

Uses ICMP Address Mask Requests for host discovery.

## **Nmap Option**

\-PM

## **ICMP Types**

| Request | Reply |
| ----- | ----- |
| Type 17 | Type 18 |

## **Example**

nmap \-PM \-sn 10.200.6.0/24

## **Notes**

* Often blocked by firewalls  
* Less reliable than other ICMP methods  
* Sends multiple requests per target

# **Question and Answers**

### **What option tells Nmap to use ICMP Timestamp requests?**

Answer:

\-PP

### **What option tells Nmap to use ICMP Address Mask requests?**

Answer:

\-PM

### **What option tells Nmap to use ICMP Echo requests?**

Answer:

\-PE

# **Summary**

## **Common Nmap Host Discovery Options**

| Option | Purpose |
| ----- | ----- |
| \-sn | Host discovery only |
| \-PR | ARP Scan |
| \-PE | ICMP Echo Scan |
| \-PP | ICMP Timestamp Scan |
| \-PM | ICMP Address Mask Scan |

## **Key Takeaways**

* ARP scans work only on local subnets.  
* ICMP scans can identify live hosts remotely.  
* Firewalls may block ICMP packets.  
* Nmap uses different discovery methods depending on privileges and network location.  
* Host discovery saves time before performing deeper scans.

OVERVIEW: NMAP HOST DISCOVERY AND NETWORK RECONNAISSANCE TECHNIQUES

This writeup covers how Nmap identifies live hosts across a network using different discovery techniques such as ARP, ICMP, TCP, and UDP probes. It also explains how target specification works and how subnetting affects scanning behavior. These concepts form the foundation of network reconnaissance before performing deeper port and service enumeration.

KEY TAKEAWAYS

• Nmap allows flexible target specification using single IPs, ranges, subnets, or input files  
 • Host discovery (ping scanning) helps identify live systems before full port scanning  
 • The use of \-sn prevents port scanning and limits results to live host detection only  
 • Different network conditions require different discovery methods (ICMP, TCP, UDP, ARP)  
 • ARP scanning is the most reliable method but only works inside the same local subnet  
 • ICMP-based methods are commonly blocked by firewalls, requiring alternative probes  
 • TCP SYN and ACK scans are useful when ICMP is restricted  
 • UDP discovery relies on ICMP unreachable responses to confirm host availability  
 • Reverse DNS (-R) can provide hostname context but may slow scanning  
 • Disabling DNS (-n) improves speed and reduces noise during scanning

DETAILED NOTES AND CONCEPTS

• Network segments are physical (switch/Wi-Fi), while subnets are logical IP groupings managed by routers  
 • Subnets define how large a network is (e.g., /24 ≈ 256 IPs, /16 ≈ 65,000 IPs)  
 • Nmap uses different host discovery techniques depending on privileges and network location  
 • Privileged users (root/sudo) can use more advanced packet types such as ARP and TCP ACK  
 • Unprivileged users rely more on basic TCP handshake methods for discovery

HOST DISCOVERY METHODS

• ARP Scan (-PR)

* Works only within local subnet  
* Fast and highly reliable  
* Uses MAC address resolution

• ICMP Echo (-PE)

* Standard ping method  
* Frequently blocked by firewalls

• ICMP Timestamp (-PP)

* Alternative to echo request  
* Less commonly filtered than echo

• ICMP Address Mask (-PM)

* Rarely supported in modern networks  
* Often blocked or ignored

• TCP SYN Ping (-PS)

* Sends SYN packets to target ports  
* Detects host availability via SYN/ACK or RST responses

• TCP ACK Ping (-PA)

* Sends ACK packets  
* Requires elevated privileges  
* RST response indicates host is alive

• UDP Ping (-PU)

* Sends UDP packets to trigger ICMP errors  
* Useful when TCP and ICMP are filtered

ADDITIONAL INFORMATION

• Nmap first identifies live hosts, then proceeds with port scanning unless disabled  
 • Multiple discovery methods increase accuracy in filtered or secured networks  
 • Combining techniques is important in real-world penetration testing  
 • Tools like ARP are effective in internal reconnaissance after initial access  
 • Masscan follows a similar principle but focuses on high-speed scanning

Q/A SECTION

What is the first IP address Nmap would scan if you provided 10.10.12.13/29 as your target?  
 10.10.12.8

How many IP addresses will Nmap scan if you provide the following range: 10.10.0-255.101-125?  
 6400

How many hosts are found alive after scanning the CONNECTION\_IP/24?  
 1

What is the option required to tell Nmap to use ICMP Timestamp to discover live hosts?  
 \-PP

What is the option required to tell Nmap to use ICMP Address Mask to discover live hosts?  
 \-PM

What is the option required to tell Nmap to use ICMP Echo to discover live hosts?  
 \-PE

Which TCP ping scan does not require a privileged account?  
 TCP SYN Ping

Which TCP ping scan requires a privileged account?  
 TCP ACK Ping

What option do you need to add to Nmap to run a TCP SYN ping scan on the telnet port?  
 \-PS23

We want Nmap to issue a reverse DNS lookup for all the possible hosts on a subnet, hoping to get some insights from the names. What option should we add?  
 \-R

