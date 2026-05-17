## **Overview: Nmap Service Detection with \-sV**

Nmap’s service version detection (\-sV) goes beyond identifying open ports by actively probing services to determine what software is running behind each port. Unlike basic scans that only show whether a port is open, \-sV attempts to extract **banner information and version details** by interacting with the service directly.

Because version detection requires communication with the service, Nmap performs a full TCP connection rather than a stealth SYN scan. This makes results more accurate but also more “visible” on the network compared to \-sS.

The intensity of detection can be adjusted using \--version-intensity, where lower levels perform lighter probing and higher levels perform deeper, more aggressive fingerprinting.

---

## **Key Takeaways**

* \-sV identifies **service name \+ version information** on open ports.  
* It performs a **full TCP handshake**, unlike stealth scans (\-sS).  
* Version detection is based on **service banners and probing responses**, not guesswork.  
* \--version-light (level 2\) performs faster, less intrusive detection.  
* Higher intensity levels improve accuracy but increase scan time and noise.  
* Some services may still show **no version information** if they do not expose banners or respond inconsistently.

---

## **Important Concept**

Service detection is not passive. Unlike port discovery, Nmap must **actively communicate with services** to retrieve version data, which is why results can vary depending on service configuration and exposure.

---

## **Questions and Answers**

What is the detected version for port 143?

Dovecot imapd

Which service did not have a version detected with \--version-light?

rpcbind

## **Overview: Nmap OS Detection (\-O) and Traceroute**

Nmap’s OS detection feature (\-O) identifies a target’s operating system by analyzing low-level TCP/IP behavior, including packet responses, TTL values, sequence patterns, and protocol quirks. Instead of relying on banners or services, it builds a **TCP/IP fingerprint** and compares it against a database of known OS signatures.

Because modern systems (especially virtualized or cloud-based ones) often modify or obscure network behavior, OS detection does not always produce an exact match. In such cases, Nmap provides a *best guess* based on similarity to known fingerprints.

To improve reconnaissance further, Nmap also supports \--traceroute, which maps the network path between the scanner and target by identifying intermediate hops using TTL-based probing.

---

## **Key Takeaways**

* \-O enables **OS fingerprinting** using TCP/IP stack behavior.  
* OS detection does not rely on services or banners.  
* Results may be **inexact due to filtering, virtualization, or cloud networks**.  
* Nmap may still infer the OS family even without a perfect match.  
* Common indicators include:  
  * TTL values (Linux ≈ 64, Windows ≈ 128\)  
  * TCP sequence behavior  
  * Network response patterns  
* \--traceroute maps intermediate routers between attacker and target.  
* Nmap traceroute differs from traditional tools by using **decrementing TTL probes**.

---

## **Core Idea**

OS detection is probabilistic, not absolute. Nmap combines multiple network-level signals to estimate the operating system, but environmental factors can reduce accuracy. Therefore, OS fingerprinting should always be used alongside other reconnaissance techniques.

---

## **Question and Answer**

Per nmap, which one is the closest OS match after running with \-O against MACHINE\_IP?

Linux

## **Overview: Nmap Scripting Engine (NSE) and HTTP/SSH Enumeration**

Nmap’s Scripting Engine (NSE) extends its scanning capabilities by allowing automated scripts written in Lua to perform deeper reconnaissance tasks. These scripts go beyond basic port scanning and service detection by interacting with services, extracting configuration details, checking for misconfigurations, and identifying known vulnerabilities.

Scripts are organized into categories such as auth, discovery, vuln, and safe, and are stored locally in /usr/share/nmap/scripts. Users can run all default scripts using \-sC or target specific ones using \--script.

NSE makes Nmap not just a scanner, but a **modular reconnaissance and vulnerability assessment tool**.

---

## **Key Takeaways**

* NSE (Nmap Scripting Engine) runs **Lua-based automation scripts** during scans.  
* Scripts are stored in /usr/share/nmap/scripts.  
* \-sC runs default scripts (safe and commonly used checks).  
* \--script "name" runs specific scripts.  
* Script categories help classify behavior (safe, vuln, brute, etc.).  
* NSE can:  
  * Extract service metadata  
  * Detect vulnerabilities  
  * Enumerate configurations  
  * Gather system fingerprints  
* Some scripts are intrusive and must be used carefully.

---

## **Important Concept**

NSE transforms Nmap from a simple port scanner into a **service intelligence tool**, capable of actively probing applications (like HTTP and SSH) to extract hidden configuration details and potential security weaknesses.

---

## **Questions and Answers**

What does the script http-robots.txt check for?

disallowed entries

Can you figure out the name for the script that checks for the remote code execution vulnerability MS15-034 (CVE-2015-1635)?

http-vuln-cve2015-1635

On the AttackBox, run Nmap with the default scripts \-sC against MACHINE\_IP. What is the http-title value?

Welcome to nginx on Debian\!

Based on its description, what is the name of the server host key algorithm that relies on SHA2-512 and is supported by MACHINE\_IP?

rsa-sha2-512 

**OVERVIEW: Nmap Scan Output Formats and File Saving**

When performing network reconnaissance using Nmap, saving scan results is essential for documentation, analysis, and future reference. Without proper file management, repeated scans become difficult to track, especially in larger environments. Nmap provides multiple output formats that serve different purposes depending on how the results will be used.

---

**Key Takeaways**

* Saving scan results helps maintain organized penetration testing reports.  
* Choosing the correct output format improves analysis efficiency.  
* Nmap supports multiple output formats for different workflows.  
* Grepable and XML formats are especially useful for automation and parsing.  
* Poor format choice (like script kiddie output) reduces usability and professionalism.

---

**Nmap Output Formats**

* **Normal Output (-oN)**  
  * Similar to terminal output  
  * Human-readable format  
  * Best for quick review and manual reporting  
* **Grepable Output (-oG)**  
  * Designed for filtering using tools like grep  
  * Each line contains complete host and port data  
  * Useful for handling multiple scan results efficiently  
* **XML Output (-oX)**  
  * Structured format for machine processing  
  * Commonly used in scripts, reporting tools, and databases  
  * Can be combined with other formats using \-oA  
* **Script Kiddie Output (-oS)**  
  * Obfuscated and stylized output  
  * Not practical for analysis  
  * Mainly for novelty or demonstration

---

**Additional Facts**

* You can save all major formats at once using:  
  * \-oA filename  
* XML output is the most flexible for integration with external tools.  
* Grepable output is optimized for command-line filtering but is harder to read manually.  
* Proper file naming conventions help manage multiple scan reports effectively.

---

**QUESTION & ANSWER**

What parameter is used to save the output in a greppable format? Write with a dash (-).  
 \-oG

Is it possible to save Nmap output in XML format (yea/nay)?  
 Yea

**Conclusion**

In this room, we learned how to detect running services, their versions, and the host operating system. We learned how to enable traceroute and how to select one or more scripts to aid in penetration testing. Finally, we covered the different formats for saving scan results for future reference. The table below summarises the most important options we covered in this room.

| Option | Meaning |
| :---- | :---- |
| \-sV | Determine service/version info on open ports |
| \-sV \--version-light | Try the most likely probes (2) |
| \-sV \--version-all | Try all available probes (9) |
| \-O | Detect OS |
| \--traceroute | Run traceroute to the target |
| \--script=SCRIPTS | Nmap scripts to run |
| \-sC or \--script=default | Run default scripts |
| \-A | Equivalent to \-sV \-O \-sC \--traceroute |
| \-oN | Save output in normal format |
| \-oG | Save output in a grepable format |
| \-oX | Save output in XML format |
| \-oA | Save output in normal, XML and Grepable formats |

