## **Overview: Network Ports, Services, TCP Flags, and Nmap Scanning**

Network ports are essential identifiers that allow communication with specific services running on a host. Just as an IP address identifies a device on a network, a port number identifies a service on that device. This concept is foundational in networking, system administration, and penetration testing because it determines how applications are accessed and discovered.

---

## **Key Takeaways**

* A **port** identifies a specific service running on a host (e.g., web server, DNS, SSH).  
* Only **one service can bind to a port per IP address** at a time.  
* Common default ports:  
  * HTTP → TCP 80  
  * HTTPS → TCP 443  
  * SSH → TCP 22  
  * DNS → UDP 53  
* A service is typically associated with a **protocol and a port number**.  
* Security tools like Nmap help identify open services and their states.  
* Firewalls can change how ports appear during scanning, even if services are running.

---

## **Port States (Nmap Perspective)**

Nmap recognizes **6 port states**, which are crucial during reconnaissance:

* **Open**  
  * A service is actively listening on the port.  
* **Closed**  
  * No service is listening, but the port is reachable.  
* **Filtered**  
  * Firewall or security device is blocking access; state cannot be determined.  
* **Unfiltered**  
  * Port is reachable, but open/closed state is unclear (common in ACK scans).  
* **Open|Filtered**  
  * Cannot distinguish between open or filtered.  
* **Closed|Filtered**  
  * Cannot distinguish between closed or filtered.

👉 The most important state for penetration testers is **Open**, as it indicates an active service that may be exploitable.

---

## **TCP Flags (Important for Scanning & Connections)**

TCP communication uses control flags inside the header to manage connections:

* **URG** – Marks urgent data  
* **ACK** – Acknowledges received data  
* **PSH** – Pushes data immediately to application  
* **RST** – Resets a connection  
* **SYN** – Starts a TCP connection (3-way handshake)  
* **FIN** – Ends a connection

👉 These flags are heavily used in port scanning techniques to analyze how a host responds.

---

## **TCP Connect Scan (-sT)**

A TCP connect scan is one of the simplest scanning methods used by Nmap:

* Performs a **full TCP 3-way handshake**:  
  1. SYN → Client initiates connection  
  2. SYN/ACK → Server responds if port is open  
  3. ACK → Connection is completed  
* After confirmation, the connection is immediately closed using RST.  
* Useful when the user **does not have root privileges**.  
* Scans the **most common 1000 ports by default**.  
* Closed ports respond with **RST/ACK**.  
* Open ports complete the handshake successfully.

---

## **Additional Facts**

* UDP port 53 is commonly used by **DNS**.  
* TCP port 22 is commonly used by **SSH**.  
* Port scanning behavior can be influenced by firewalls and filtering rules.  
* Option \-F in Nmap reduces scan scope to the top 100 ports.  
* Option \-r scans ports sequentially instead of randomly.

---

## **Questions and Answers**

Which service uses UDP port 53 by default?  
 DNS

Which service uses TCP port 22 by default?  
 SSH

How many port states does Nmap consider?  
 6

Which port state is the most interesting to discover as a pentester?  
 Open

What 3 letters represent the Reset flag?  
 RST

Which flag needs to be set when you initiate a TCP connection (first packet of TCP 3-way handshake)?  
 SYN

What is the state of the FTP service running on port 21?  
 open

What is Nmap’s guess about the service running on port 53?  
 domain

## **Overview: TCP SYN Scanning, UDP Scanning, and Nmap Optimization**

This section explains advanced network scanning techniques used to discover open services on a target system. It focuses on how TCP SYN scans, UDP scans, and scan customization options work in practice using Nmap.

---

## **Key Takeaways**

* Privileged users (root/sudo) can perform **stealthier SYN scans (-sS)**.  
* SYN scans do **not complete the TCP 3-way handshake**, making them harder to detect than connect scans.  
* Unprivileged users are limited to **TCP connect scans (-sT)**.  
* UDP scanning works differently because UDP is **connectionless**.  
* Scan behavior can be heavily controlled using timing, rate, and parallelism options.  
* Different scan types reveal different levels of network visibility.

---

## **TCP SYN Scan (-sS)**

A SYN scan is one of the most common and efficient scanning methods used by Nmap.

### **How it works**

* Sends a **SYN packet** to the target port.  
* If the port is open:  
  * Target responds with **SYN/ACK**  
  * Scanner immediately sends **RST** (no full connection is completed)  
* If closed:  
  * Target responds with **RST/ACK**

### **Why it is important**

* Does **not complete TCP handshake**  
* Generates **less logging activity**  
* Faster and more stealthy than TCP connect scans

### **Observed behavior**

* Open ports discovered without establishing a full connection  
* Closed ports behave similarly to connect scans

---

## **UDP Scanning (-sU)**

UDP scanning works differently from TCP because UDP is **connectionless**.

### **How it works**

* Sends a UDP packet to target port  
* If port is:  
  * **Closed** → ICMP “port unreachable” is returned  
  * **Open** → often **no response at all**

### **Key limitation**

* Lack of response does not guarantee the port is open (it may be filtered)

### **Example findings**

* Port 53 (DNS) often appears open  
* Port 161 (SNMP) may appear closed or filtered depending on responses

---

## **Port Scanning Customization in Nmap**

Nmap allows fine-tuned control over scans:

### **Port selection options**

* \-p22,80,443 → scan specific ports  
* \-p1-1023 → scan a range  
* \-p5000-5500 → scan custom range  
* \-p- → scan all 65535 TCP ports  
* \-F → scan top 100 common ports

---

### **Timing and performance control**

* \-T0 → paranoid (very slow, stealthy)  
* \-T1 → sneaky  
* \-T2 → polite  
* \-T3 → normal (default)  
* \-T4 → aggressive (fast, common in labs/CTFs)  
* \-T5 → insane (fast but less accurate)

---

### **Traffic control options**

* \--max-rate \<n\> → limits packets per second  
* \--min-rate \<n\> → ensures minimum packet rate  
* \--min-parallelism \<n\> → sets minimum concurrent probes  
* \--max-parallelism \<n\> → limits concurrency

Example:

* \--min-parallelism=64 ensures at least 64 probes run in parallel

---

## **Additional Facts**

* SYN scan is the **default scan mode for privileged users**.  
* UDP scans are useful for detecting services like **DNS (53)** and **SNMP (161)**.  
* Closed UDP ports typically respond with **ICMP type 3, code 3** messages.  
* Stealth scans reduce detection but still depend on firewall behavior.  
* Faster scans increase risk of packet loss and inaccurate results.

---

## **Questions and Answers**

After launching a TCP SYN scan, how many SYN-ACK packets are successfully received in AttackBox?  
 4

How many ports are open on the target machine?  
 4

What is the state of port number 161 over UDP in the target machine?  
 closed

What is the service name according to Nmap on port 161?  
 snmp

What is the option to scan all the TCP ports between 5000 and 5500?  
 \-p5000-5500

How can you ensure that Nmap will run at least 64 probes in parallel?  
 \--min-parallelism=64

What option would you add to make Nmap very slow and paranoid?  
 \-T0

