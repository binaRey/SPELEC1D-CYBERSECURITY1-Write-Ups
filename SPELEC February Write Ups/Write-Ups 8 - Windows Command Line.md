# **Windows Command Line — TryHackMe Write-Up**

**Author:** Reynan S. Cortado  
**Platform:** TryHackMe  
**Topic:** Command Line

---

# **Introduction**

The **Command Line Interface (CLI)** is a powerful way to interact with operating systems without using a graphical interface. While beginners often prefer GUIs, professionals in **cybersecurity, system administration, and networking** rely heavily on command-line tools because they are **faster, more efficient, and easier to automate**.

In this module, we explore the **Windows Command Prompt**, its commands, and how it can be used to:

* Retrieve system information

* Troubleshoot network issues

* Manage files and directories

* Monitor and manage running processes

The default command-line interpreter in Windows environments is:

cmd.exe

Understanding these commands is critical for **penetration testers, SOC analysts, and system administrators**.

---

# **Tools & Operating Systems**

## **Tools Used**

* **Windows Command Prompt (cmd.exe)**

* Built-in networking tools

* Built-in process management tools

## **Operating Systems**

* **Windows Server 2022**

* **Windows 10**

These environments simulate real-world enterprise systems used in organizations.

---

### **Quick Summary**

* **cmd.exe** is the default Windows CLI interpreter

* Used for **system diagnostics, networking, and administration**

* Widely used in **cybersecurity labs and enterprise environments**

---

# **Remote Access (SSH & RDP in Labs)**

Although Windows primarily uses **RDP (Remote Desktop Protocol)** for graphical remote access, cybersecurity labs and Linux systems often use **SSH**.

## **What is SSH?**

**Secure Shell (SSH)** is a secure protocol used for **remote command-line access to systems over a network**.

It encrypts communication to protect against:

* credential theft

* packet sniffing

* man-in-the-middle attacks

Common in **Linux servers and cloud environments**.

---

## **SSH Connection Example**

ssh username@target-ip

Example:

ssh admin@10.10.10.5

If a specific port is used:

ssh username@target-ip \-p 2222  
---

### **Why SSH Matters in Cybersecurity**

* Used in **remote server management**

* Used in **penetration testing labs**

* Often targeted in **brute-force attacks**

---

💡 **Tip**

Always disable password authentication on production servers and use **SSH keys** instead.

---

### **Quick Summary**

* SSH allows **secure remote CLI access**

* Used heavily in **Linux systems**

* Important skill for **cybersecurity professionals**

---

# **Command Flags & Documentation**

Command flags (also called **switches**) modify the behavior of commands.

In Windows, flags typically start with:

/

Example:

ipconfig /all

This shows **detailed network configuration information**.

---

## **Viewing Command Help**

You can display help information using:

command /?

Example:

ipconfig /?

Or use:

help  
---

## **Example Commands with Flags**

dir /a

Shows **hidden files**.

dir /s

Lists files in **subdirectories**.

tasklist /FI "imagename eq notepad.exe"

Filters processes.

---

### **Quick Summary**

* Flags modify command behavior

* Windows flags use /

* /? shows help documentation

---

# **Basic System Information**

Windows provides commands for retrieving system information quickly.

## **OS Version**

ver

Example result:

10.0.20348.2655  
---

## **Detailed System Information**

systeminfo

Displays:

* OS version

* system architecture

* installed memory

* system uptime

* domain membership

---

## **Driver Information**

driverquery | more

Shows installed drivers **one page at a time**.

---

## **Useful Utility Commands**

help

Displays available commands.

cls

Clears the command screen.

---

### **Quick Summary**

* ver → OS version

* systeminfo → detailed system info

* driverquery → driver list

* cls → clears terminal

---

# **Network Troubleshooting**

Networking commands are essential for diagnosing connectivity issues.

---

## **Network Configuration**

### **Basic Network Information**

ipconfig

Displays:

* IP address

* subnet mask

* default gateway

---

### **Detailed Network Information**

ipconfig /all

Shows:

* MAC address

* DNS servers

* DHCP status

* adapter details

Example lab answer:

Gateway IP: 10.10.0.1  
---

💡 **Tip**

Security analysts often use **ipconfig /all** to identify:

* network interfaces

* DNS servers

* MAC addresses

---

## **Connectivity Testing**

### **Ping**

ping google.com

Tests connectivity by sending **ICMP packets**.

---

### **Traceroute**

tracert google.com

Displays **each network hop** between your system and the target.

---

## **DNS Lookup**

nslookup example.com

Resolves a **domain name to its IP address**.

---

## **Active Network Connections**

netstat \-abon

Displays:

* active connections

* listening ports

* associated processes

Example:

Port 3389 → TermService

Port **3389** is used by **Remote Desktop Protocol**.

---

### **Quick Summary**

* ipconfig → network configuration

* ping → connectivity testing

* tracert → path to destination

* nslookup → DNS resolution

* netstat → active connections

---

# **File and Directory Management**

Managing files is one of the most common command-line tasks.

---

## **Working with Directories**

### **Show Current Directory**

cd

### **Change Directory**

cd folder\_name  
---

### **List Files**

dir  
---

### **Show Hidden Files**

dir /a  
---

### **Recursive Directory Listing**

dir /s  
---

### **Create Directory**

mkdir newfolder  
---

### **Remove Directory**

rmdir foldername  
---

### **View Directory Structure**

tree  
---

## **Working with Files**

### **Display File Contents**

type file.txt  
---

### **View File Page by Page**

more file.txt  
---

### **Copy Files**

copy file.txt destination

Example:

copy \*.md backup\_folder  
---

### **Move Files**

move file.txt new\_location  
---

### **Delete Files**

del file.txt

or

erase file.txt

⚠️ **FYI**

File deletion using del is **permanent** and cannot be undone easily.

---

### **Quick Summary**

* dir → list files

* mkdir → create directory

* copy → duplicate files

* move → relocate files

* del → permanently delete files

---

# **Process & Task Management**

Monitoring running programs is essential in system administration and security.

---

## **View Running Processes**

tasklist

Displays all active processes.

---

## **Filter Specific Processes**

tasklist /FI "imagename eq notepad.exe"  
---

## **Terminate a Process**

taskkill /PID 1516

This stops a process using its **Process ID (PID)**.

---

💡 **Tip**

Malware analysts often use tasklist to identify suspicious processes.

---

### **Quick Summary**

* tasklist → view processes

* tasklist /FI → filter processes

* taskkill → terminate processes

---

# **System Maintenance Commands**

Additional commands useful for system diagnostics:

chkdsk

Checks disk for errors.

---

sfc /scannow

Scans and repairs corrupted system files.

---

driverquery

Lists installed device drivers.

---

# **System Shutdown Commands**

Shutdown commands allow administrators to control system power states.

### **Shutdown System**

shutdown /s  
---

### **Restart System**

shutdown /r  
---

### **Abort Shutdown**

shutdown /a

Stops a scheduled shutdown.

---

### **Quick Summary**

* /s → shutdown

* /r → restart

* /a → abort shutdown

---

# **Command Cheat Sheet**

| Command | Flags | Description |
| ----- | ----- | ----- |
| ver | — | Displays OS version |
| systeminfo | — | Detailed system information |
| ipconfig | /all | Full network configuration |
| dir | /a | Show hidden files |
| dir | /s | Recursive listing |
| netstat | \-abon | Show network connections |
| tasklist | /FI | Filter running processes |
| taskkill | /PID | Terminate process |
| shutdown | /r | Restart system |

---

# **Key Beginner Insights**

Important lessons from this module:

* CLI is **faster and more efficient than GUI**

* Networking troubleshooting relies heavily on **CLI tools**

* Understanding processes helps detect **malware or suspicious activity**

* Many cybersecurity tools operate through the **command line**

---

# **Personal Learning Notes**

Key takeaways from the lab:

* Using **ipconfig /all** to identify MAC addresses and gateways

* Identifying services listening on ports using **netstat**

* Managing processes using **tasklist and taskkill**

* Navigating directories efficiently through CLI

---

# **Next Steps / Recommended Learning**

After mastering Windows CLI basics, continue learning:

* PowerShell administration

* Linux command line

* Network troubleshooting

* Active Directory fundamentals

Good practice platforms:

* TryHackMe

* Hack The Box

---

# **Final Thoughts**

Mastering the **command line** is a critical skill in cybersecurity. Whether you're performing **incident response, penetration testing, or system administration**, the ability to navigate and control systems through CLI tools will significantly increase your efficiency and technical capability.

The more you practice using these commands, the more **natural and powerful** they will become.

