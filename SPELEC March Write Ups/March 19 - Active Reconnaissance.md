# **Web Browser Reconnaissance**

## **Overview**

Web browsers are essential tools for passive reconnaissance and web application analysis. Beyond simply viewing websites, browsers can reveal valuable information about a target’s infrastructure, technologies, cookies, scripts, and hidden resources. Using built-in Developer Tools and browser extensions, security professionals can inspect client-side behavior and gather intelligence without directly attacking the target.

## **Key Concepts**

### **Browser Communication Ports**

Web browsers communicate using specific default ports:

| Protocol | Default Port |
| ----- | ----- |
| HTTP | 80 |
| HTTPS | 443 |

Custom ports may also be used, such as:

https://127.0.0.1:8834/

This connects to localhost using HTTPS on port 8834.

---

## **Developer Tools**

Developer Tools allow analysts to inspect and modify website components directly from the browser.

### **Common Shortcut Keys**

| Browser | Shortcut |
| ----- | ----- |
| Windows/Linux | Ctrl \+ Shift \+ I |
| macOS | Option \+ Command \+ I |

### **Capabilities of Developer Tools**

* Inspect HTML structure  
* Analyze CSS styling  
* View and edit JavaScript files  
* Monitor network requests and responses  
* Inspect cookies and session data  
* Discover hidden files and directories  
* Debug client-side behavior

Developer Tools are extremely valuable during reconnaissance because they provide insight into how a web application functions internally.

---

## **Useful Browser Extensions**

### **FoxyProxy**

Allows rapid switching between proxy servers.

#### **Common Uses**

* Integrating with Burp Suite  
* Routing traffic through testing proxies  
* Quickly changing network routes

---

### **User-Agent Switcher and Manager**

Lets users spoof different browsers or operating systems.

#### **Example**

Pretending to browse from:

* iPhone  
* Android  
* Chrome  
* Safari

This can help identify mobile-specific functionality or alternate site behavior.

---

### **Wappalyzer**

Detects technologies used by websites.

#### **Information Revealed**

* Web frameworks  
* Programming languages  
* CMS platforms  
* Analytics tools  
* Server technologies

This helps attackers and defenders identify potential technologies and attack surfaces.

---

## **Question and Answer**

### **Question:**

Using Developer Tools, what is the total number of questions on the website?

### **Answer:**

8  
---

# **Ping**

## **Overview**

The ping command checks whether a remote system is reachable over the network. It works by sending ICMP Echo Request packets and waiting for ICMP Echo Reply packets.

Ping is commonly used to:

* Verify network connectivity  
* Confirm a host is online  
* Measure response latency  
* Detect packet loss

---

## **Basic Syntax**

### **Linux/macOS**

ping MACHINE\_IP

### **Windows**

ping MACHINE\_IP  
---

## **Limiting Packet Count**

### **Linux**

ping \-c 10 MACHINE\_IP

### **Windows**

ping \-n 10 MACHINE\_IP  
---

## **ICMP Basics**

Ping uses the Internet Control Message Protocol (ICMP).

### **Important ICMP Types**

| Type | Description |
| ----- | ----- |
| 8 | Echo Request |
| 0 | Echo Reply |

---

## **Sample Successful Ping**

ping \-c 5 MACHINE\_IP

### **Example Output**

5 packets transmitted, 5 received, 0% packet loss

### **Interpretation**

* Target is online  
* Network path is functioning  
* ICMP traffic is not blocked

---

## **Sample Failed Ping**

Destination Host Unreachable

### **Possible Causes**

* Target machine is offline  
* Firewall blocks ICMP  
* Network disconnection  
* Router issue  
* System crash

---

## **Important Facts**

### **ICMP Header Size**

The ICMP header size is:

8 bytes  
---

### **Windows Firewall Behavior**

Microsoft Windows Firewall blocks ping requests by default.

---

## **Question and Answer**

### **Question:**

Which option sets the size of the ICMP echo request data?

### **Answer:**

\-s  
---

### **Question:**

What is the size of the ICMP header in bytes?

### **Answer:**

8  
---

### **Question:**

Does Microsoft Windows Firewall block ping by default?

### **Answer:**

Y  
---

### **Question:**

How many ping replies were received after running:

ping \-c 10 MACHINE\_IP

### **Answer:**

10  
---

# **Traceroute**

## **Overview**

The traceroute command identifies the path packets take between two systems by discovering intermediate routers (hops).

It helps analysts:

* Map network routes  
* Identify routing issues  
* Measure latency between hops  
* Discover network infrastructure

---

## **Command Syntax**

### **Linux/macOS**

traceroute MACHINE\_IP

### **Windows**

tracert MACHINE\_IP  
---

## **How Traceroute Works**

Traceroute manipulates the TTL (Time To Live) value in IP packets.

### **TTL Basics**

Each router decreases TTL by 1\.

When TTL reaches 0:

* Router discards packet  
* Router sends ICMP Time Exceeded message

Traceroute gradually increases TTL values to identify each router in the path.

---

## **Important Notes**

### **Routes Can Change**

Packets may follow different paths depending on:

* Network congestion  
* Dynamic routing  
* ISP decisions

---

### **Some Routers Hide Information**

Certain routers:

* Ignore traceroute requests  
* Do not send ICMP responses  
* Appear as \* in output

---

## **Example Findings**

### **Traceroute A**

* Total hops observed: 14  
* Final IP before destination:

172.67.69.208  
---

### **Traceroute B**

* Total hops observed: 26  
* Final IP before destination:

104.26.11.229  
---

## **Key Observations**

### **Traceroute Reveals**

* Number of routers between systems  
* Router IP addresses  
* Response times  
* Possible routing changes

### **Traceroute Limitations**

* Some routers suppress replies  
* Paths may vary over time  
* Firewalls may block responses

---

## **Question and Answer**

### **Question:**

In Traceroute A, what is the IP address of the last router/hop before reaching tryhackme.com?

### **Answer:**

172.67.69.208  
---

### **Question:**

In Traceroute B, what is the IP address of the last router/hop before reaching tryhackme.com?

### **Answer:**

104.26.11.229  
---

### **Question:**

In Traceroute B, how many routers are between the two systems?

### **Answer:**

26  
---

## **Summary**

This section introduced essential reconnaissance techniques using:

* Web browsers  
* Developer Tools  
* Browser extensions  
* Ping  
* Traceroute

These tools provide valuable information about:

* Web technologies  
* Host availability  
* Network routing  
* Infrastructure layout

Understanding these tools is fundamental for both penetration testers and defenders during the reconnaissance and enumeration phases of cybersecurity assessments.

# **TELNET Protocol**

## **Overview**

TELNET (Teletype Network) is a protocol developed in 1969 that allows remote communication with a system through a command-line interface (CLI). It operates over TCP port 23 by default and was commonly used for remote administration before more secure protocols became standard.

Unlike modern secure protocols, TELNET sends all transmitted data in cleartext, including usernames and passwords. Because of this, attackers monitoring network traffic can easily capture sensitive credentials. The modern and secure replacement for TELNET is SSH (Secure Shell).

Although insecure for administration, the Telnet client remains useful in penetration testing and reconnaissance because it can connect to any TCP service and display service banners.

## **Key Concepts**

* Uses TCP protocol  
* Default port: 23  
* Sends data in plaintext  
* Vulnerable to packet sniffing and credential theft  
* Useful for banner grabbing and service enumeration  
* Can interact with services like:  
  * HTTP  
  * SMTP  
  * POP3  
  * FTP

## **Basic TELNET Syntax**

telnet MACHINE\_IP PORT

Example:

telnet 192.168.1.10 80

## **Using TELNET for Banner Grabbing**

TELNET can manually communicate with web servers using HTTP commands.

Example:

telnet MACHINE\_IP 80

Then send:

GET / HTTP/1.1  
host: telnet

After pressing Enter twice, the server responds with headers and webpage content.

Example response:

HTTP/1.1 200 OK  
Server: nginx/1.6.2

## **Important Information Gathered**

From the response:

Server: nginx/1.6.2

We can identify:

* Web server software: nginx  
* Server version: 1.6.2

This process is called banner grabbing and is useful for:

* Service identification  
* Version enumeration  
* Vulnerability assessment

## **Question and Answers**

### **What is the name of the running server?**

Answer:

Apache

### **What is the version of the running server?**

Answer:

2.4.61

# **Netcat (nc)**

## **Overview**

Netcat, commonly called nc, is a versatile networking utility capable of:

* Connecting to TCP/UDP ports  
* Listening on ports  
* Transferring data  
* Banner grabbing  
* Creating simple chat connections  
* Supporting penetration testing workflows

Because of its flexibility, Netcat is often referred to as the “Swiss Army Knife” of networking.

## **Key Features**

* Supports TCP and UDP  
* Can act as:  
  * Client  
  * Server  
* Useful for:  
  * Port testing  
  * Banner grabbing  
  * File transfers  
  * Reverse shells  
  * Basic communication channels

## **Connecting to a Service**

Syntax:

nc MACHINE\_IP PORT

Example:

nc MACHINE\_IP 80

Then send:

GET / HTTP/1.1  
host: netcat

Possible response:

HTTP/1.1 200 OK  
Server: nginx/1.6.2

## **Running Netcat in Listen Mode**

Server-side:

nc \-vnlp 1234

Equivalent:

nc \-lvnp 1234

## **Netcat Options**

| Option | Meaning |
| ----- | ----- |
| \-l | Listen mode |
| \-p | Specify port |
| \-n | Disable DNS resolution |
| \-v | Verbose output |
| \-vv | Very verbose |
| \-k | Keep listening after disconnect |

## **Notes**

* Port numbers below 1024 require root privileges  
* \-n avoids DNS lookups  
* \-v helps with debugging and visibility

## **Example Communication Setup**

### **Listener**

nc \-vnlp 1234

### **Client**

nc MACHINE\_IP 1234

Anything typed on either side will appear on the other side.

## **Question and Answers**

### **What is the version of the running server on port 21?**

Answer:

0.17

# **Summary**

## **Reconnaissance Tools Covered**

| Tool | Purpose |
| ----- | ----- |
| ping | Check if target is online |
| traceroute | Trace packet path |
| telnet | Connect to TCP services |
| netcat | Flexible networking utility |
| web browser | Gather web application information |

## **Common Commands**

### **Ping**

Linux/macOS:

ping \-c 10 MACHINE\_IP

Windows:

ping \-n 10 MACHINE\_IP

### **Traceroute**

Linux/macOS:

traceroute MACHINE\_IP

Windows:

tracert MACHINE\_IP

### **Telnet**

telnet MACHINE\_IP PORT

### **Netcat Client**

nc MACHINE\_IP PORT

### **Netcat Server**

nc \-lvnp PORT

# **Additional Notes**

* TELNET is outdated and insecure due to plaintext communication.  
* SSH should always be preferred for secure remote administration.  
* Netcat is heavily used in penetration testing because of its flexibility.  
* Banner grabbing helps identify vulnerable software versions.  
* Reconnaissance is a critical first step in penetration testing and system assessment.

