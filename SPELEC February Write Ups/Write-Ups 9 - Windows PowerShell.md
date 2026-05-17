# **Windows PowerShell — TryHackMe Write-Up**

**Author:** Reynan S. Cortado  
**Platform:** TryHackMe  
**Topic**: Command Line

---

# **Introduction**

**PowerShell** is a powerful command-line shell and scripting language designed for **system administration, automation, and cybersecurity operations**.

Unlike the traditional Windows command interpreter **cmd.exe**, PowerShell is built using an **object-oriented architecture** based on the **.NET framework**, allowing it to handle structured data rather than plain text.

This module introduces the fundamentals of PowerShell and teaches how to:

* Understand PowerShell’s architecture

* Use **cmdlets** (command-lets)

* Navigate and manage the filesystem

* Filter and manipulate data

* Retrieve system and network information

* Analyze processes and services

* Automate tasks with scripts

PowerShell is widely used in **penetration testing, system administration, and incident response**.

---

# **Tools & Operating Systems**

## **Tools Used**

* **PowerShell**

* Windows command utilities

* Built-in PowerShell cmdlets

## **Operating Systems**

* **Windows Server 2022**

* **Windows 10**

These environments simulate **enterprise Windows infrastructures** used in cybersecurity labs.

---

### **Quick Summary**

* **PowerShell** is a powerful automation shell

* Built on **.NET object-oriented architecture**

* Used extensively in **administration and cybersecurity**

---

# **Remote Access / SSH**

Although PowerShell is primarily used locally on Windows systems, administrators and security professionals often manage systems remotely using **SSH or PowerShell Remoting**.

## **What is SSH?**

**Secure Shell (SSH)** is a cryptographic protocol used for **secure remote command-line access**.

It encrypts communication to prevent:

* credential interception

* network sniffing

* man-in-the-middle attacks

---

## **SSH Connection Example**

ssh username@target-ip

Example:

ssh admin@10.10.10.10

Using a custom port:

ssh username@target-ip \-p 2222  
---

💡 **Tip**

Security professionals frequently use SSH to connect to **Linux servers, cloud systems, and lab environments**.

---

### **Quick Summary**

* **SSH enables remote command-line access**

* Common in **Linux administration and cybersecurity labs**

* Communication is **encrypted**

---

# **What is PowerShell?**

From Microsoft’s official documentation:

PowerShell is a cross-platform task automation solution made up of a command-line shell, scripting language, and configuration management framework.

PowerShell is:

* **Object-oriented**

* Built on the **.NET framework**

* Cross-platform (Windows, Linux, macOS)

---

## **CMD vs PowerShell**

| Feature | CMD | PowerShell |
| ----- | ----- | ----- |
| Data type | Text | Objects |
| Automation | Limited | Advanced |
| Language support | Basic | Full scripting language |
| Framework | None | .NET |

Example:

CMD command:

dir

PowerShell equivalent:

Get-ChildItem  
---

### **Key Concept**

PowerShell follows a **Verb-Noun naming convention**.

Example:

Get-Content  
Set-Location  
New-Item  
---

### **Quick Summary**

* PowerShell is **object-oriented**

* Uses **cmdlets** with Verb-Noun structure

* More powerful than traditional **CMD**

---

# **PowerShell Basics**

PowerShell commands are called **cmdlets (command-lets)**.

## **Common Cmdlets**

Get-Content

Reads file contents (similar to cat in Linux or type in CMD).

Set-Location

Changes directory (similar to cd).

Get-Command

Lists all available commands.

---

## **Example Tasks**

### **List Commands Starting With "Remove"**

Get-Command \-Name Remove\*  
---

### **Display Output**

Write-Output Hello

Equivalent alias:

echo  
---

### **View Command Examples**

Get-Help New-LocalUser \-examples  
---

💡 **Tip**

Use Get-Help frequently when learning PowerShell commands.

---

### **Quick Summary**

* PowerShell commands are **cmdlets**

* Use **Verb-Noun naming**

* Get-Help displays documentation

---

# **Command Flags & Documentation**

PowerShell uses **parameters** rather than traditional flags.

Example:

Get-Command \-Name Remove\*

Here:

* Get-Command → cmdlet

* \-Name → parameter

---

## **Getting Command Documentation**

Get-Help Get-Process

View examples:

Get-Help Get-Process \-examples  
---

## **Useful Discovery Commands**

Get-Command

Lists available commands.

Get-Alias

Shows command aliases.

Example:

Get-Alias \-Name echo\*  
---

### **Quick Summary**

* PowerShell uses **parameters instead of flags**

* Get-Help provides documentation

* Get-Command helps discover cmdlets

---

# **Filesystem Management**

PowerShell includes powerful cmdlets for **file and directory management**.

---

## **List Directory Contents**

Get-ChildItem

Equivalent to:

* dir (CMD)

* ls (Linux)

Example:

Get-ChildItem \-Path C:\\Users

Result from the lab:

4 items displayed  
---

## **Change Directory**

Set-Location C:\\Users

Equivalent to cd.

---

## **Create File or Directory**

New-Item file.txt  
---

## **Delete Files or Folders**

Remove-Item file.txt

⚠️ **FYI**

Deleting files with Remove-Item is **permanent** unless backups exist.

---

## **Read File Content**

Get-Content file.txt

Example:

Get-Content big-treasure.txt

Flag discovered in lab:

THM{p34rlInAsh3ll}  
---

### **Quick Summary**

* Get-ChildItem → list files

* Set-Location → change directory

* New-Item → create file/folder

* Remove-Item → delete file

* Get-Content → read file

---

# **Piping, Filtering, and Sorting Data**

PowerShell allows **powerful data manipulation** using pipes.

Pipe symbol:

|

It sends the output of one command to another.

---

## **Filtering Data**

Example:

Get-ChildItem | Where-Object \-Property Length \-gt 100

Lists files larger than **100 bytes**.

---

## **Comparison Operators**

| Operator | Meaning |
| ----- | ----- |
| **\-eq** | equal to |
| **\-ne** | not equal |
| **\-gt** | greater than |
| **\-ge** | greater than or equal |
| **\-lt** | less than |
| **\-le** | less than or equal |

---

## **Sorting Results**

Sort-Object  
---

## **Selecting Specific Properties**

Select-Object

Example:

Get-Service | Select-Object Name, DisplayName  
---

💡 **Tip**

Filtering output is extremely useful when analyzing **large datasets or system logs**.

---

### **Quick Summary**

* PowerShell pipes **objects**, not text

* Where-Object filters results

* Sort-Object sorts output

* Select-Object refines displayed data

---

# **System and Network Information**

PowerShell provides detailed information about the system.

---

## **System Information**

Get-ComputerInfo

Displays:

* OS details

* hardware information

* system configuration

---

## **Network Configuration**

Get-NetIPConfiguration

Equivalent to:

ipconfig  
---

## **Local User Accounts**

Get-LocalUser

Example user discovered:

p1r4t3

Description:

A merry life and a short one.  
---

### **Quick Summary**

* Get-ComputerInfo → system details

* Get-NetIPConfiguration → network configuration

* Get-LocalUser → list local users

---

# **Real-Time System Analysis**

PowerShell allows monitoring of system activity.

---

## **Running Processes**

Get-Process

Equivalent to:

tasklist  
---

## **Services**

Get-Service  
---

## **Network Connections**

Get-NetTCPConnection

Important property:

OwningProcess

Shows which process initiated the connection.

---

## **File Hash**

Get-FileHash \-Path big-treasure.txt

Example result:

71FC5EC11C2497A32F8F08E61399687D90ABE6E204D2964DF589543A613F3E08  
---

### **Investigating a Suspicious Service**

Get-Service | Where-Object { $\_.DisplayName \-like "\*merry\*" } | Select-Object Name, DisplayName

Result:

p1r4t3-s-compass  
---

### **Quick Summary**

* Get-Process → running processes

* Get-Service → system services

* Get-NetTCPConnection → active connections

* Get-FileHash → file integrity check

---

# **PowerShell Scripting**

PowerShell supports **task automation and remote execution**.

One powerful cmdlet is:

Invoke-Command  
---

## **Remote Command Execution**

Invoke-Command \-ComputerName RoyalFortune \-ScriptBlock {Get-Service}

This runs:

Get-Service

On a remote computer named:

RoyalFortune  
---

💡 **Tip**

PowerShell remoting is widely used in:

* enterprise system administration

* automation scripts

* penetration testing

---

### **Quick Summary**

* PowerShell supports **remote automation**

* Invoke-Command runs commands on remote machines

* Comparable to **Bash scripting in Linux**

---

# **Command Cheat Sheet**

| Command | Parameter | Description |
| ----- | ----- | ----- |
| Get-Command | \-Name | List commands matching pattern |
| Get-Help | \-examples | Show command usage examples |
| Get-ChildItem | \-Path | List directory contents |
| Where-Object | \-Property | Filter objects |
| Select-Object | — | Choose specific properties |
| Get-Process | — | List running processes |
| Get-Service | — | Show system services |
| Get-NetTCPConnection | — | Display network connections |

---

# **Important System Directories**

| Directory | Purpose |
| ----- | ----- |
| **C:\\Windows** | Core Windows operating system files |
| **C:\\Users** | User home directories |
| **C:\\Program Files** | Installed applications |
| **C:\\Temp** | Temporary files |

---

💡 **Cybersecurity Insight**

These directories are often inspected during **digital forensics, malware analysis, and privilege escalation investigations**.

---

# **Key Beginner Insights**

Important takeaways from this module:

* PowerShell is **object-oriented**, unlike CMD

* Cmdlets follow a **Verb-Noun structure**

* Piping objects enables **powerful filtering**

* PowerShell can perform **deep system analysis**

---

# **Personal Learning Notes**

Lessons learned during this lab:

* Discovering hidden users using **Get-LocalUser**

* Navigating directories with **Set-Location**

* Finding hidden files using **Get-ChildItem**

* Analyzing services and connections with PowerShell tools

These techniques are commonly used in **incident response and penetration testing**.

---

# **Next Steps / Recommended Learning**

After mastering PowerShell fundamentals, continue studying:

* PowerShell scripting

* Active Directory enumeration

* Windows privilege escalation

* Incident response automation

Practice platforms:

* TryHackMe

* Hack The Box

---

# **Final Thoughts**

PowerShell is one of the most powerful tools in the **Windows cybersecurity ecosystem**. Its ability to manipulate structured data, automate tasks, and perform deep system analysis makes it essential for:

* **System administrators**

* **Security engineers**

* **Penetration testers**

Mastering PowerShell will significantly improve your ability to **analyze, automate, and secure Windows environments**.

