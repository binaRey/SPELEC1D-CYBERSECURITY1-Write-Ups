## **Overview: Sniffing Attack and Packet Capture Analysis**

A sniffing attack is a type of network interception technique where an attacker captures and analyzes data packets traveling across a network. This becomes dangerous when data is transmitted in **cleartext (unencrypted form)**, allowing sensitive information such as login credentials, emails, and private messages to be exposed.

Attackers typically rely on packet capture tools and privileged network access to monitor traffic between systems. Once intercepted, the data can be reconstructed and read in plain form if no encryption is applied.

---

## **Key Takeaways**

* Sniffing attacks involve **capturing network packets** to extract sensitive information.  
* Cleartext communication is highly vulnerable to interception.  
* Attackers need:  
  * Access to network traffic (physical or logical)  
  * Elevated privileges (root/administrator)  
* Common interception tools include:  
  * **Tcpdump** – command-line packet analyzer  
  * **Wireshark** – graphical packet analysis tool  
  * **Tshark** – command-line version of Wireshark  
* Attacks often succeed in environments with:  
  * **Man-in-the-Middle (MITM) positioning**  
  * Misconfigured or unsecured networks

---

## **Tools Used in Sniffing**

* **Tcpdump**  
  * CLI-based packet capture tool  
  * Requires root/admin privileges  
  * Example use: filtering traffic by port (e.g., POP3 port 110\)  
* **Wireshark**  
  * GUI-based network protocol analyzer  
  * Allows filtering of traffic for easier inspection  
  * Can reveal usernames and passwords in unencrypted protocols  
* **Tshark**  
  * Terminal-based version of Wireshark  
  * Useful for automated or remote packet analysis

---

## **How Sniffing Works (Example Scenario)**

When a user accesses email via an unencrypted protocol like POP3:

* The attacker captures traffic using a command like:  
  * sudo tcpdump port 110 \-A  
* Packets are filtered to show only POP3 traffic  
* Credentials are transmitted in separate packets:  
  * Username: USER frank  
  * Password: PASS D2xc9CgD

This demonstrates how easily sensitive data can be exposed when encryption is not used.

---

## **Security Implications**

* Any **unencrypted protocol is vulnerable** to sniffing attacks.  
* Attackers only need access to traffic flow (e.g., via:  
  * Network tap  
  * Switch port mirroring  
  * MITM attack)  
* Packet inspection tools can reconstruct entire sessions.

---

## **Mitigation Strategies**

* Always use encryption protocols:  
  * **TLS/SSL** for HTTP, SMTP, IMAP, POP3  
* Replace insecure protocols:  
  * Telnet → **SSH**  
* Avoid transmitting credentials in plaintext  
* Use secure authentication mechanisms and encrypted sessions

---

## **Additional Facts**

* Sniffing does not modify traffic; it is a **passive attack** (unless combined with MITM).  
* Even CLI tools like Tcpdump can extract full credential data with proper filtering.  
* Wireshark filters allow quick isolation of protocol-specific traffic (e.g., IMAP, POP3).

---

## **Questions**

What do you need to add to the command sudo tcpdump to capture only Telnet traffic?

port 23

What is the simplest display filter you can use with Wireshark to show only IMAP traffic?

imap

## **Overview: Man-in-the-Middle (MITM) Attack**

A Man-in-the-Middle (MITM) attack happens when an attacker secretly positions themselves between two communicating parties. The victim believes they are communicating directly with a legitimate destination, but in reality, all messages are being intercepted and possibly altered by the attacker.

This allows the attacker not only to **read sensitive information** but also to **modify messages in transit**, such as changing transaction values or credentials before they reach the intended recipient.

---

## **Key Takeaways**

* MITM attacks involve an attacker intercepting communication between two parties.  
* The attacker can:  
  * Read data  
  * Modify data  
  * Inject false information  
* It often occurs when:  
  * There is no authentication between parties  
  * Data is transmitted in cleartext  
  * Integrity checks are missing  
* Commonly affected protocols:  
  * HTTP  
  * FTP  
  * SMTP  
  * POP3

---

## **How MITM Attacks Work**

* Victim (A) sends a request intended for destination (B)  
* Attacker (E) intercepts the communication  
* Attacker alters or reads the message  
* Modified message is forwarded to B without detection

Example:

* A sends: “Transfer $20 to M”  
* E modifies it to: “Transfer $2000 to M”  
* B receives the altered message and processes it

---

## **Tools Used in MITM Attacks**

* **Ettercap**  
  * Network sniffing and MITM attack tool  
  * Provides multiple interfaces for interaction and attack execution  
* **Bettercap**  
  * Advanced MITM framework  
  * Flexible tool that can be invoked in different ways depending on user needs  
  * Supports network discovery, sniffing, and manipulation

---

## **Why HTTP and Cleartext Protocols Are Risky**

* HTTP sends data without encryption  
* Attackers can easily intercept traffic  
* Users cannot detect tampering without integrity checks

Even if users trust the website visually, the actual communication can be altered in transit.

---

## **Mitigation Strategies**

* Use encrypted protocols (HTTPS instead of HTTP)  
* Implement:  
  * **Transport Layer Security (TLS)**  
  * **Public Key Infrastructure (PKI)**  
  * Trusted root certificates  
* Ensure:  
  * Authentication of both parties  
  * Message integrity (signing/hashing)  
  * Encryption of data in transit

---

## **Security Insight**

* MITM attacks are dangerous because they are often **invisible to users**  
* Success depends on breaking or bypassing trust in communication channels  
* Modern systems rely heavily on **TLS encryption** to prevent interception and tampering

---

## **Questions**

How many different interfaces does Ettercap offer?

3

In how many ways can you invoke Bettercap?

3

## **Overview: SSL/TLS and Secure Communication**

SSL (Secure Sockets Layer) and its modern successor TLS (Transport Layer Security) are cryptographic protocols designed to protect data transmitted over networks. Their main purpose is to ensure **confidentiality, integrity, and authentication** of communication between a client and a server.

Originally introduced by Netscape in 1994, SSL evolved into TLS in 1999 due to security improvements. Today, TLS is the standard used across modern internet services, while the term “SSL” is still commonly used informally.

---

## **Key Takeaways**

* SSL/TLS protects data from:  
  * Sniffing attacks  
  * Man-in-the-Middle (MITM) attacks  
* It works by encrypting data after a secure handshake between client and server.  
* TLS operates between the application layer and transport layer (often described as part of the presentation layer in the OSI model).  
* Cleartext protocols become secure when upgraded with TLS (e.g., HTTP → HTTPS).  
* Modern systems rely on **trusted certificates issued by Certificate Authorities (CAs)**.

---

## **Protocol Security Upgrades**

* HTTP → HTTPS (Port 80 → 443\)  
* FTP → FTPS (Port 21 → 990\)  
* SMTP → SMTPS (Port 25 → 465\)  
* POP3 → POP3S (Port 110 → 995\)  
* IMAP → IMAPS (Port 143 → 993\)

These secure versions use SSL/TLS to encrypt communication.

---

## **How HTTPS Works (Simplified Flow)**

1. Establish a TCP connection  
2. Establish SSL/TLS handshake  
3. Send encrypted HTTP requests

Without TLS:

* Data is sent in plaintext and can be intercepted

With TLS:

* Data is encrypted after handshake  
* Only client and server can decrypt messages

---

## **SSL/TLS Handshake (Simplified)**

* Client sends **ClientHello** (supported encryption methods)  
* Server responds with **ServerHello** \+ certificate  
* Key exchange information is shared  
* Both sides generate a shared secret key  
* Communication switches to encrypted mode

Once completed:

* All communication is encrypted using a shared session key  
* Attackers cannot read or modify traffic

---

## **Role of Certificates**

Certificates ensure that:

* The server is legitimate  
* The connection is not intercepted (prevents MITM)

They include:

* Issued-to identity (organization/domain)  
* Issuer (Certificate Authority)  
* Validity period

Browsers automatically verify certificates to ensure secure connections.

---

## **Security Insight**

* TLS prevents MITM by combining:  
  * Encryption  
  * Authentication  
  * Integrity checks  
* Even if traffic is intercepted, it appears as unreadable ciphertext  
* Trust is built through **Certificate Authorities (PKI system)**

---

## **Question**

DNS can also be secured using TLS. What is the three-letter acronym of the DNS protocol that uses TLS?

DoT

## **SSH Overview: Secure Remote Administration**

Secure Shell (SSH) is a cryptographic network protocol used to securely access and manage remote systems over an unsecured network. It ensures that all communication between client and server is protected through encryption and authentication.

---

## **Key Takeaways**

* SSH enables **secure remote system administration**  
* Provides:  
  * Server identity verification  
  * Encrypted communication (confidentiality)  
  * Message integrity (tamper detection)  
* Default service port: **22**  
* Authentication methods:  
  * Username \+ password  
  * Public/private key pairs  
* All commands and responses are transmitted through an **encrypted channel**

---

## **How SSH Works**

* Client connects using:  
  * ssh username@MACHINE\_IP  
* Server requests authentication (password or key)  
* Once verified:  
  * A secure encrypted session is established  
  * All terminal commands are protected from interception

---

## **Security Features**

* Prevents sniffing attacks by encrypting traffic  
* Protects against MITM using:  
  * Server fingerprint verification (first-time connection)  
* Ensures:  
  * Confidentiality  
  * Integrity  
  * Authentication

---

## **File Transfer with SSH (SCP)**

SSH also supports secure file transfer using **SCP (Secure Copy Protocol)**:

* Download from remote → local:  
  * scp mark@MACHINE\_IP:/remote/path/file \~  
* Upload local → remote:  
  * scp file.txt mark@MACHINE\_IP:/remote/path/

All transfers are encrypted using SSH.

---

## **Additional Insight**

* SSH is also the foundation for **SFTP (SSH File Transfer Protocol)**  
* FTP can be secured using:  
  * FTPS (SSL/TLS-based)  
  * SFTP (SSH-based, port 22\)  
* SSH is widely used in system administration, cloud servers, and DevOps environments

---

## **Questions**

Use SSH to connect to MACHINE\_IP as mark with the password XBtc49AB. Using uname \-r, find the Kernel release?

5.15.0-119-generic

Use SSH to download the file book.txt from the remote system. How many KBs did scp display as download size?

98 KB

## **Overview: Password Attacks and Credential Guessing**

Password attacks target authentication systems by attempting to discover or bypass user credentials. Since many network services rely on passwords for identity verification, attackers often focus on exploiting weak or commonly used passwords through automated or manual methods.

Authentication is based on proving identity using something the user knows (passwords or PINs), and weak password choices make systems vulnerable to unauthorized access.

---

## **Key Takeaways**

* Authentication confirms a user’s identity before granting access to a service.  
* Many services (e.g., POP3, IMAP, SSH, FTP) rely on password-based login systems.  
* Weak or common passwords are frequently reused, making them easy targets.  
* Password attacks exploit human behavior rather than technical flaws alone.

---

## **Types of Password Attacks**

* **Password Guessing**  
  * Uses personal knowledge about the target  
  * Example: pet names, birthdays, or predictable patterns  
* **Dictionary Attack**  
  * Uses a predefined list of commonly used passwords (wordlists)  
  * Example: leaked password databases like RockYou  
* **Brute Force Attack**  
  * Tries all possible combinations of characters  
  * Extremely slow but comprehensive

---

## **Automated Attack Tools**

* **THC Hydra**  
  * Supports multiple protocols (SSH, FTP, POP3, IMAP, HTTP, etc.)  
  * Automates password guessing using wordlists  
  * Can perform parallel login attempts for efficiency  
* Common usage structure:  
  * Target service \+ username \+ wordlist \+ protocol

---

## **Common Weak Password Trends**

From real-world breaches:

* “123456”, “123456789”, “12345678”  
* “password”  
* “qwerty”  
* “111111”  
* Simple keyboard patterns remain widely used despite awareness

---

## **Mitigation Strategies**

* Enforce strong password policies (length \+ complexity)  
* Account lockout after multiple failed attempts  
* Delay login responses (rate limiting)  
* Use CAPTCHA for automated bot prevention  
* Enable Two-Factor Authentication (2FA)  
* Use certificate-based or key-based authentication where possible

---

## **Security Insight**

* Password attacks are effective because users often choose predictable credentials.  
* Automation tools significantly increase attack speed and success rate.  
* Defenses rely heavily on **limiting repeated attempts and enforcing strong authentication policies**.

We learned that one of the email accounts is lazie. What is the password used to access the IMAP service on MACHINE\_IP? 

Answer: Butterfly

**Conclusion**  
This room covered various protocols, their usage, and how they work under the hood. Three common attacks are:

1. Sniffing Attack  
2. MITM Attack  
3. Password Attack

For each of the above, we focused both on the attack details and the mitigation steps.

Many other attacks can be conducted against specific servers and protocols. We will provide a list of some related modules.

* [Vulnerability Research](https://tryhackme.com/module/vulnerability-research): This module provides more information about vulnerabilities and exploits.  
* [Metasploit](https://tryhackme.com/module/metasploit): This module trains you on how to use Metasploit to exploit target systems.  
* [Burp Suite](https://tryhackme.com/module/learn-burp-suite): This module teaches you how to use Burp Suite to intercept HTTP traffic and launch attacks related to the web.

It is good to remember the default port number for common protocols. For convenience, the services we covered are listed in the following table sorted by alphabetical order.

| Protocol | TCP Port | Application(s) | Data Security |
| :---- | :---- | :---- | :---- |
| FTP | 21 | File Transfer | Cleartext |
| FTPS | 990 | File Transfer | Encrypted |
| HTTP | 80 | Worldwide Web | Cleartext |
| HTTPS | 443 | Worldwide Web | Encrypted |
| IMAP | 143 | Email (MDA) | Cleartext |
| IMAPS | 993 | Email (MDA) | Encrypted |
| POP3 | 110 | Email (MDA) | Cleartext |
| POP3S | 995 | Email (MDA) | Encrypted |
| SFTP | 22 | File Transfer | Encrypted |
| SSH | 22 | Remote Access and File Transfer | Encrypted |
| SMTP | 25 | Email (MTA) | Cleartext |
| SMTPS | 465 | Email (MTA) | Encrypted |
| Telnet | 23 | Remote Access | Cleartext |

Hydra remains a very efficient tool that you can launch from the terminal to try the different passwords. We summarize its main options in the following table.

| Option | Explanation |
| :---- | :---- |
| \-l username | Provide the login name |
| \-P WordList.txt | Specify the password list to use |
| server service | Set the server address and service to attack |
| \-s PORT | Use in case of non-default service port number |
| \-V or \-vV | Show the username and password combinations being tried |
| \-d | Display debugging output if the verbose output is not helping |

