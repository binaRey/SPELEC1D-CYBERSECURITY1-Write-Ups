## **Overview: TCP Stealth Scanning Techniques (Null, FIN, and Xmas Scans in Nmap)**

This section explains three **stealth TCP scan types used in Nmap**—Null scan, FIN scan, and Xmas scan—which operate by manipulating TCP flags instead of completing a full connection handshake. These scans are useful for probing systems behind stateless firewalls, but they are inherently unreliable because they infer port status based on the absence of responses rather than explicit confirmations.

All three techniques rely on the same core principle: **open or filtered ports do not respond**, while closed ports typically return an RST packet. This difference allows inference of port states without performing a standard SYN scan.

However, modern stateful firewalls often block or silently drop these packets, making results ambiguous and frequently labeled as open|filtered.

---

## **Key Takeaways**

* Stealth scans avoid full TCP handshakes by manipulating TCP flags.  
* Port states are inferred from responses, not directly confirmed.  
* Null, FIN, and Xmas scans behave similarly in output.  
* open|filtered means no response was received (ambiguous state).  
* Closed ports typically respond with an RST packet.  
* Stateless firewalls may allow these scans to bypass detection.  
* Stateful firewalls often block or neutralize these scan types.  
* These scans are more useful for evasion than for precise enumeration.

---

## **Scan Types Explained**

### **Null Scan (-sN)**

* Sets **no TCP flags (0 flags set to 1\)**.  
* Open or filtered ports do not respond.  
* Closed ports send an RST packet.  
* Results are ambiguous for open vs filtered states.

---

### **FIN Scan (-sF)**

* Sends a TCP packet with only the **FIN flag set**.  
* Open ports do not respond.  
* Closed ports respond with RST.  
* Behavior closely mirrors Null scan results.

---

### **Xmas Scan (-sX)**

* Sets **FIN \+ PSH \+ URG flags simultaneously**.  
* Packet appears “lit up” like a Christmas tree (hence the name).  
* Open ports remain silent.  
* Closed ports return RST.  
* Output is similar to Null and FIN scans.

---

## **Behavioral Insight**

* All three scans rely on **lack of response \= potential open or filtered port**.  
* Response patterns depend heavily on firewall configuration.  
* Stateless firewalls may fail to detect these unusual flag combinations.  
* Stateful firewalls typically block or normalize such packets.  
* These scans are unreliable for precise port enumeration but useful for stealth probing.

---

## **Core Idea**

Stealth scans manipulate TCP flags to bypass simple firewall detection rules, but they trade accuracy for evasion. Since results depend on missing responses rather than confirmed replies, interpretation must always consider the possibility of filtering.

---

## **Questions and Answers**

In a null scan, how many flags are set to 1?

0

In a FIN scan, how many flags are set to 1?

1

In a Xmas scan, how many flags are set to 1?

3

Launch a FIN scan against the target VM. How many ports appear as open|filtered?

9

Repeat your scan launching a null scan against the target VM. How many ports appear as open|filtered?

9

## **Overview: Maimon Scan in TCP Stealth Scanning (Nmap \-sM)**

The **Maimon scan** was first described by Uriel Maimon in 1996 and is a lesser-known TCP stealth scanning technique used in Nmap via the \-sM option. It operates by sending a TCP packet with a specific combination of flags set to probe how a target system responds to unusual packet structures.

Unlike modern scans commonly used in penetration testing, the Maimon scan has limited effectiveness against most contemporary systems. However, it is valuable for understanding how TCP implementations behave under non-standard conditions and how early scanning techniques attempted to bypass firewall logic.

In this scan, the behavior of open and closed ports is often indistinguishable, making it unreliable for identifying active services on most modern networks.

---

## **Key Takeaways**

* The Maimon scan was introduced in 1996 by Uriel Maimon.  
* It is executed using the \-sM option in Nmap.  
* It relies on TCP packets with specific flag combinations.  
* Most modern systems respond the same way for open and closed ports.  
* Because responses are indistinguishable, it often detects no open ports.  
* It is primarily educational rather than practical in modern environments.  
* Helps demonstrate how different TCP flag behaviors affect scanning results.

---

## **Maimon Scan Behavior**

* A TCP packet is sent with a **special combination of flags set**.  
* Target systems typically respond with an **RST packet regardless of port state**.  
* This uniform response prevents reliable port state detection.  
* As a result, scans often report all ports as closed or inconclusive.

---

## **Core Idea**

The Maimon scan demonstrates that **not all TCP flag combinations produce useful or distinguishable responses across systems**. While it is not commonly used in real-world reconnaissance today, it remains important for understanding how TCP stack behavior can influence scan accuracy and detection methods.

---

## **Questions and Answers**

In the Maimon scan, how many flags are set?

2

## **Overview: TCP ACK, Window, and Custom Flag Scanning in Nmap**

This section explains advanced TCP scanning techniques used in Nmap to analyze firewall behavior rather than directly identifying open services. Unlike SYN-based scans, **ACK and Window scans do not reliably determine whether a port is open or closed**. Instead, they are primarily used to map firewall rules and detect which ports are filtered or unfiltered.

These scans work by sending specially crafted TCP packets and analyzing responses—usually RST packets—to infer how a target system or firewall is handling traffic.

---

## **Key Takeaways**

* ACK and Window scans are used to analyze firewall behavior, not service discovery.  
* Responses are typically RST packets regardless of port state.  
* These scans help distinguish **filtered vs unfiltered ports**, not open vs closed ones.  
* Window scans may sometimes reveal additional nuance based on TCP window size.  
* Custom scans allow arbitrary TCP flag combinations for deeper testing.  
* Firewall configuration heavily influences scan output.  
* Unfiltered ports do not guarantee that a service is running.

---

## **Scan Types Explained**

### **TCP ACK Scan (-sA)**

* Sends a TCP packet with only the **ACK flag set**.  
* Used to map firewall rules.  
* Response behavior:  
  * RST \= port is unfiltered  
  * No response \= port is filtered  
* Does NOT indicate whether a service is running.

---

### **TCP Window Scan (-sW)**

* Similar to ACK scan but also examines the **TCP Window field** in RST responses.  
* In some systems, this helps infer additional state information.  
* On many modern systems, results are similar to ACK scans.  
* Primarily useful for detecting firewall filtering differences.

---

### **Custom TCP Scan (--scanflags)**

* Allows manual selection of TCP flags.  
* Example combination: SYN \+ RST \+ FIN.  
* Used for experimentation and advanced evasion techniques.  
* Requires understanding of TCP behavior for correct interpretation.  
* Focuses on behavioral differences rather than service detection.

---

## **Behavioral Insight**

* ACK scan reveals whether a port is filtered or unfiltered.  
* Window scan may show differences in firewall responses.  
* Custom scans allow deeper probing of TCP stack behavior.  
* Firewalls influence results more than services themselves in these scans.  
* These techniques are best suited for **security assessment of filtering rules**, not enumeration.

---

## **Core Idea**

ACK, Window, and custom scans shift focus from identifying services to understanding **how a firewall treats traffic**. They expose network filtering logic rather than application-layer availability, making them valuable for defensive analysis and security auditing.

---

## **Questions and Answers**

In TCP Window scan, how many flags are set?

1

You decided to experiment with a custom TCP scan that has the reset flag set. What would you add after \--scanflags?

RST

Launch an ACK scan against the target VM with the firewall enabled. How many ports appear unfiltered?

5

What is the new port number that appeared?

443

Is there any service behind the newly discovered port number? (yea/nay)

yea

## **Overview: IP and MAC Spoofing with Nmap (Decoys and Source Forgery)**

This section explains how **Nmap can manipulate source identity information** during a scan by spoofing IP and MAC addresses or using decoy systems. These techniques are primarily used for **obfuscation and traffic analysis testing**, not reliable reconnaissance, because they require specific network conditions to function correctly.

When spoofing is used, the target system responds to the forged source address rather than the attacker’s real IP. This means the attacker must be able to **observe or capture return traffic**, otherwise the scan results become meaningless.

Decoy scanning further complicates detection by making the scan appear to originate from multiple sources, blending the attacker’s real IP among fake ones.

---

## **Key Takeaways**

* Nmap can spoof source IP addresses using the \-S option.  
* Spoofed scans require traffic visibility to capture responses.  
* Without packet capture access, results are unreliable or useless.  
* MAC spoofing (\--spoof-mac) only works on local networks (same LAN/WiFi).  
* Decoy scanning (\-D) hides the real attacker among fake sources.  
* These techniques are used for obfuscation, not accurate service discovery.  
* Network topology determines whether spoofing techniques will work.

---

## **Spoofed IP Scanning**

### **Basic Concept**

* The attacker sends packets with a forged source IP.  
* The target responds to the spoofed IP address.  
* The attacker must intercept or observe responses to analyze results.

### **Nmap Command Structure**

* Spoof source IP:  
  * nmap \-S SPOOFED\_IP MACHINE\_IP  
* Requires:  
  * Correct network interface (\-e)  
  * No ping requirement (\-Pn)  
  * Traffic capture capability

---

## **Decoy Scanning**

### **Concept**

* Multiple fake source IPs are added alongside the real attacker.  
* Makes scan traffic appear distributed across several hosts.  
* Obscures the true origin of the scan.

### **Behavior**

* Target sees multiple source IPs.  
* Responses are sent to all listed sources.  
* Real attacker blends into noise.

---

## **Core Idea**

Spoofing and decoy techniques manipulate perceived scan origin to **obscure attacker identity and confuse network attribution**, but they only work effectively when the attacker can still observe or capture the network responses.

---

## **Questions and Answers**

What do you need to add to the command sudo nmap MACHINE\_IP to make the scan appear as if coming from the source IP address 10.10.10.11 instead of your IP address?

\-S 10.10.10.11

What do you need to add to the command sudo nmap MACHINE\_IP to make the scan appear as if coming from the source IP addresses 10.10.20.21 and 10.10.20.28 in addition to your IP address?

\-D 10.10.20.21,10.10.20.28,ME

## **Overview: Firewall Evasion, IDS Detection, and Packet Fragmentation in Nmap**

Firewalls and Intrusion Detection Systems (IDS) are designed to monitor, filter, and analyze network traffic to prevent unauthorized access and detect malicious behavior. Firewalls primarily control traffic based on rules applied to IP and transport layer headers, while IDS systems go further by inspecting packet payloads for known attack signatures or suspicious patterns.

To evade detection, attackers may attempt to manipulate how packets are transmitted. One such technique is **packet fragmentation**, where large packets are split into smaller pieces so that security systems may fail to fully reconstruct and analyze them in real time. This can reduce the likelihood of detection in less advanced or improperly configured IDS/firewall systems.

However, modern security tools are often capable of reassembling fragmented packets, making this technique less reliable today. Still, it remains important for understanding how low-level network evasion techniques work.

---

## **Key Takeaways**

* Firewalls filter traffic based on rules applied to IP and transport headers.  
* IDS systems inspect payloads for malicious patterns or signatures.  
* Fragmentation splits packets into smaller pieces to bypass inspection.  
* Nmap supports fragmentation using \-f, \-ff, or \--mtu.  
* Smaller fragments may evade weak inspection systems.  
* Modern IDS tools can often reassemble fragmented packets.  
* Packet fragmentation affects how data is transmitted, not its content.  
* This technique is more educational than reliably effective today.

---

## **Packet Fragmentation Behavior**

### **Single Fragment Level (-f)**

* Splits data into **8-byte fragments**.  
* Each fragment contains part of the TCP segment plus IP headers.  
* Useful for basic evasion attempts.

### **Double Fragment Level (-ff)**

* Splits data into **16-byte fragments**.  
* Each fragment can contain larger portions of the TCP segment.  
* Reduces number of total fragments compared to \-f.

---

## **Core Idea**

Packet fragmentation attempts to exploit limitations in traffic inspection systems by breaking payloads into smaller units. While historically useful for evasion, modern security systems often mitigate this by reassembling fragments before analysis.

---

## **Questions and Answers**

If the TCP segment has a size of 64, and the \-ff option is being used, how many IP fragments will you get?

4

## **Overview: Idle Scan (Zombie Scan) and IP ID-Based Stealth Reconnaissance**

The **Idle Scan**, also known as the **Zombie Scan**, is an advanced Nmap technique that allows an attacker to scan a target indirectly by using a third-party host as a proxy. Instead of sending packets directly to the target, the attacker spoofs the IP address of an “idle” system (the zombie), making it appear as though the zombie is performing the scan.

This technique relies on a key behavior in some systems: the **IP Identification (IP ID) field**, which increments with each packet sent. By carefully observing changes in the zombie’s IP ID values, an attacker can infer whether target ports are open, closed, or filtered—without directly interacting with the target.

However, this method only works if the zombie host is truly idle and predictable in its IP ID sequence.

---

## **Key Takeaways**

* Idle scan uses a third-party host (zombie) to perform indirect scanning.  
* It relies on predictable **IP ID increment behavior**.  
* The attacker never directly communicates with the target system.  
* Port state is inferred by comparing IP ID changes on the zombie.  
* Works only if the zombie system is idle and predictable.  
* If the zombie is active or randomizing IP IDs, results become unreliable.  
* It is one of the stealthiest scanning techniques in Nmap.

---

## **How Idle Scan Works (Simplified Flow)**

* Step 1: Probe zombie → record initial IP ID.  
* Step 2: Send spoofed SYN to target (appears to come from zombie).  
* Step 3: Probe zombie again → record new IP ID.  
* Step 4: Compare values:  
  * \+1 increase → port closed/filtered  
  * \+2 increase → port open (extra response caused increment)

---

## **Core Idea**

Idle scanning exploits predictable IP ID behavior in a third-party system to **indirectly map target ports without revealing the attacker’s identity**, making it one of the most stealth-oriented reconnaissance techniques in TCP scanning.

---

## **Questions and Answers**

You discovered a rarely-used network printer with the IP address 10.10.5.5, and you decide to use it as a zombie in your idle scan. What argument should you add to your Nmap command?

*\-sI 10.10.5.5* 

## **Overview: Understanding Nmap \--reason, Verbosity, and Scan Interpretation**

Nmap normally reports whether a host or port is open, closed, or filtered without explaining *why* it reached that conclusion. The \--reason flag enhances scan output by showing the **exact packet-level evidence** behind each decision.

This is especially useful in TCP SYN scans (\-sS), where different responses such as SYN-ACK, RST, or no reply determine port states. Instead of just showing results, Nmap explains the logic behind them, making the scan output more transparent and easier to analyze during security assessments.

Verbosity flags (\-v, \-vv) and debugging flags (\-d, \-dd) further increase output detail, showing step-by-step scan execution, packet transmission, and response handling.

---

## **Key Takeaways**

* \--reason explains *why* Nmap marks a port as open, closed, or filtered.  
* Open ports are typically identified by receiving a **SYN-ACK response**.  
* Closed ports usually respond with an **RST packet**.  
* \--reason improves transparency in scan interpretation.  
* \-v and \-vv increase output detail (scan progress and findings).  
* \-d and \-dd provide deep debugging information about scan internals.  
* These options are useful for learning, troubleshooting, and auditing firewall behavior.

---

## **How Nmap Determines Port States**

* **Open port → SYN-ACK received**  
* **Closed port → RST received**  
* **Filtered port → no response or blocked packet**

With \--reason, Nmap explicitly shows this logic in the output instead of only labeling the result.

---

## **Core Idea**

The \--reason flag bridges the gap between **scan results and packet-level behavior**, allowing users to understand exactly how Nmap interprets network responses rather than treating results as black-box outputs.

---

## **Questions and Answers**

Use Nmap with nmap \-sS \-F \--reason MACHINE\_IP to scan the VM. What is the reason provided for the stated port(s) being open?

syn-ack

**Conclusion**  
This room covered the following types of scans.

| Port Scan Type | Example Command |
| :---- | :---- |
| TCP Null Scan | sudo nmap \-sN MACHINE\_IP |
| TCP FIN Scan | sudo nmap \-sF MACHINE\_IP |
| TCP Xmas Scan | sudo nmap \-sX MACHINE\_IP |
| TCP Maimon Scan | sudo nmap \-sM MACHINE\_IP |
| TCP ACK Scan | sudo nmap \-sA MACHINE\_IP |
| TCP Window Scan | sudo nmap \-sW MACHINE\_IP |
| Custom TCP Scan | sudo nmap \--scanflags URGACKPSHRSTSYNFIN MACHINE\_IP |
| Spoofed Source IP | sudo nmap \-S SPOOFED\_IP MACHINE\_IP |
| Spoofed MAC Address | \--spoof-mac SPOOFED\_MAC |
| Decoy Scan | nmap \-D DECOY\_IP,ME MACHINE\_IP |
| Idle (Zombie) Scan | sudo nmap \-sI ZOMBIE\_IP MACHINE\_IP |
| Fragment IP data into 8 bytes | \-f |
| Fragment IP data into 16 bytes | \-ff |

| Option | Purpose |
| :---- | :---- |
| \--source-port PORT\_NUM | Specify source port number |
| \--data-length NUM | Append random data to reach the given length |

These scan types rely on setting TCP flags in unexpected ways to prompt ports for a reply. Null, FIN, and Xmas scans provoke a response from closed ports, while Maimon, ACK, and Window scans provoke a response from open and closed ports.

| Option | Purpose |
| :---- | :---- |
| \--reason | explains how Nmap made its conclusion |
| \-v | verbose |
| \-vv | very verbose |
| \-d | debugging |
| \-dd | more details for debugging |

