## **Overview: Metasploit Framework, Modules, Exploits, and Payloads**

Metasploit Framework is one of the most widely used tools in penetration testing, offering a complete suite for reconnaissance, exploitation, and post-exploitation. It provides modular functionality that allows security professionals to identify vulnerabilities, develop exploits, and gain control over target systems in a structured way.

---

## **Key Takeaways**

* Metasploit Framework is used across the entire penetration testing lifecycle.  
* It is divided into **modules**, each serving a specific purpose (exploitation, scanning, payload delivery, etc.).  
* The main interface is **msfconsole**, a command-line tool used to interact with the framework.  
* Understanding **exploits, vulnerabilities, and payloads** is essential for using Metasploit effectively.  
* Payload structure determines how an attack is delivered and executed on a target system.

---

## **Core Concepts**

### **Exploit**

* Code that takes advantage of a **vulnerability** in a target system.  
* Allows attackers to trigger unintended behavior or gain access.

### **Vulnerability**

* A flaw in system design, logic, or implementation.  
* Can lead to unauthorized access or information disclosure if exploited.

### **Payload**

* Code executed on the target after a successful exploit.  
* Determines the attacker’s goal (e.g., shell access, command execution, backdoor).

---

## **Metasploit Modules**

Metasploit Framework organizes functionality into several module types:

---

### **Auxiliary Modules**

* Used for supporting tasks like:  
  * Scanning  
  * Fuzzing  
  * Crawling  
  * Information gathering  
* Do not directly exploit vulnerabilities.

---

### **Encoders**

* Encode payloads to avoid detection by signature-based antivirus systems.  
* Limited effectiveness against modern security solutions.

---

### **Evasion Modules**

* Designed specifically to bypass antivirus and security mechanisms.  
* More advanced than encoders, but success is not guaranteed.

---

### **Exploits**

* Organized by target platform (Windows, Linux, Android, etc.).  
* Actively trigger vulnerabilities to gain access.

---

### **NOPs (No Operation)**

* Instructions that do nothing for one CPU cycle.  
* Used to stabilize payload sizes and memory alignment.

---

### **Payloads**

Payloads are the code executed on the target system after exploitation.

#### **Types of Payloads**

* **Adapters**  
  * Convert payloads into different formats (e.g., PowerShell).  
* **Singles**  
  * Fully self-contained payloads.  
  * Do not require additional components.  
* **Stagers**  
  * Small initial payloads that establish a connection.  
  * Download the full payload later.  
* **Stages**  
  * Full-featured payloads delivered by stagers.

---

### **Post Modules**

* Used after successful exploitation.  
* Focus on:  
  * Privilege escalation  
  * Credential harvesting  
  * System enumeration  
  * Maintaining access

---

## **Additional Facts**

* The main interface is launched using msfconsole.  
* Payloads can be **single (inline)** or **staged**, depending on execution flow.  
* Staged payloads reduce initial payload size for stealth and reliability.  
* Self-contained payloads are easier to execute but less flexible.  
* Metasploit Framework is commonly used in both ethical hacking and vulnerability research.

---

## **Questions and Answers**

What is the name of the code taking advantage of a flaw on the target system?  
 Exploit

What is the name of the code that runs on the target system to achieve the attacker's goal?  
 Payload

What are self-contained payloads called?  
 Singles

Is "windows/x64/pingback\_reverse\_tcp" among singles or staged payload?  
 Singles

## **Overview: Metasploit Console (msfconsole), Contexts, Modules, and Session Management**

Metasploit Framework uses **msfconsole** as its primary command-line interface. It provides a structured environment for interacting with exploits, payloads, and post-exploitation tools, while also supporting Linux command execution and session management within the console.

---

## **Key Takeaways**

* **msfconsole** is the main interface of Metasploit Framework.  
* It supports:  
  * Module selection and configuration  
  * Exploit execution  
  * Session handling  
  * Basic Linux commands  
* The framework operates in **contexts**, meaning settings apply only to the selected module unless set globally.  
* Successful exploitation creates a **session**, which allows interaction with the target system.  
* Proper parameter configuration is required before launching any exploit.

---

## **Metasploit Console Basics**

### **Launching msfconsole**

* Start using:  
  * msfconsole  
* The prompt changes to:  
  * msf6 \> (or similar version)

### **Capabilities inside msfconsole**

* Run Linux commands (e.g., ls, ping)  
* Use Metasploit-specific commands  
* Navigate modules and sessions

---

## **Important Console Concepts**

### **Context-Based Operation**

Metasploit Framework works in different contexts:

* **No context (msf6 \>)**  
  * General command environment  
* **Module context (msf6 exploit/... \>)**  
  * Settings apply only to selected module  
* **Session context (meterpreter \>)**  
  * Active connection to a target system

👉 Switching modules resets non-global settings.

---

## **Core Commands in msfconsole**

### **Navigation & Control**

* use → Select a module  
* back → Exit module context  
* help → Show command usage  
* history → View previous commands

---

### **Module Inspection**

* show options → Displays required parameters  
* show payloads → Lists compatible payloads  
* info → Detailed module information

---

### **Search Function**

* search \<keyword\> → Finds modules  
* Can filter by:  
  * CVE  
  * service (e.g., apache)  
  * type (exploit, auxiliary)

---

## **Setting Parameters**

### **Local settings**

* set RHOSTS 10.10.x.x → target IP  
* set LHOST 10.10.x.x → attacker IP  
* set LPORT 4444 → listener port  
* set PAYLOAD ... → choose payload

### **Global settings**

* setg RHOSTS 10.10.x.x  
  * Applies across all modules

### **Clearing values**

* unset PAYLOAD → remove single value  
* unset all → reset module settings  
* unsetg → clear global settings

---

## **Exploitation Process**

### **Running an exploit**

* exploit → launches attack  
* run → alias for exploit

### **Background execution**

* exploit \-z → run and automatically background session

### **Optional check mode**

* Some modules support check  
  * Verifies vulnerability without exploiting it

---

## **Sessions in Metasploit**

A session is a **live connection to a compromised system**.

### **Session commands**

* sessions → list active sessions  
* sessions \-i \<id\> → interact with session  
* background or CTRL+Z → return to msfconsole

---

## **Command Prompt Types**

* **System shell**  
  * root@machine:\#  
* **msfconsole**  
  * msf6 \>  
* **Module context**  
  * msf6 exploit/... \>  
* **Meterpreter**  
  * meterpreter \>  
* **Target shell**  
  * C:\\Windows\\system32\>

---

## **Additional Facts**

* msfconsole supports **tab completion** for faster navigation.  
* Output redirection (e.g., \> file.txt) is **not supported** inside msfconsole.  
* Settings depend on context unless defined globally.  
* Multiple sessions can run simultaneously.  
* Meterpreter provides advanced post-exploitation control over a target system.

---

## **Questions and Answers**

How would you set the LPORT value to 6666?  
 set LPORT 6666

How would you set the global value for RHOSTS to 10.10.19.23?  
 setg RHOSTS 10.10.19.23

What command would you use to clear a set payload?  
 unset PAYLOAD

What command do you use to proceed with the exploitation phase?  
 exploit

