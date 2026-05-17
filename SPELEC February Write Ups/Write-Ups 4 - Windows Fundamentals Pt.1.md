# **Windows Fundamentals Pt.1 — TryHackMe Write-Up**

**Author:** Reynan S. Cortado  
**Platform:** TryHackMe  
**Topic:** Windows & Active Directory Fundamentals

---

# **Introduction**

This guide summarizes **core Windows operating system fundamentals** that are important for **cybersecurity learners, system administrators, and IT students**.

Understanding Windows internals helps security professionals:

* Identify **system misconfigurations**

* Investigate **malware persistence**

* Analyze **file systems and user privileges**

* Navigate **critical operating system directories**

This study guide focuses on:

* Windows versions and editions

* Windows file systems (NTFS, FAT)

* Important system directories

* User accounts and privileges

* Security protections such as **UAC**

* Key administrative tools like **Task Manager** and **Control Panel**

---

# **Tools & Operating Systems**

## **Operating Systems Discussed**

* Windows XP

* Windows Vista

* Windows 7

* Windows 10

* Windows 11

### **Windows Edition Differences**

Example:

* **Windows 11 Pro**

  * Supports **BitLocker encryption**

  * Enterprise and business security features

* **Windows 11 Home**

  * Does **NOT support BitLocker**

### **Security Insight**

Encryption like **BitLocker** protects data if a device is **lost or stolen**.

---

### **Quick Summary**

* Windows has evolved through several major versions.

* Different **editions** offer different **security capabilities**.

* Professional editions typically support **advanced encryption**.

---

# **Windows File Systems**

Modern Windows operating systems primarily use the **NTFS file system**.

## **Previous Windows File Systems**

| File System | Description |
| ----- | ----- |
| **FAT16** | Early Windows file system with limited storage support |
| **FAT32** | Improved FAT system, still used in removable devices |
| **HPFS** | Used in older IBM/Windows systems |
| **NTFS** | Modern Windows file system with advanced security |

---

## **NTFS Features**

NTFS introduced several improvements:

* **Supports files larger than 4GB**

* **File and folder permissions**

* **File compression**

* **Encryption (EFS)**

* **Alternate Data Streams (ADS)**

### **Alternate Data Streams (ADS)**

ADS allows a file to contain **multiple streams of hidden data**.

Example concept:

file.txt:hiddenstream

This is particularly important in **cybersecurity investigations** because:

* Malware may **hide data inside files**

* Hidden streams can be used for **covert storage**

---

### **FYI**

USB drives and SD cards often still use **FAT32**, but most Windows systems use **NTFS**.

---

### **Quick Summary**

* **NTFS is the modern Windows file system**

* Supports **permissions, encryption, and large files**

* ADS can hide data and is important in **security investigations**

---

# **Windows System Directories**

## **Windows Directory**

C:\\Windows

This folder contains **core Windows operating system files**.

However, Windows **does not strictly need to be installed on the C drive**.

---

## **Environment Variables**

Windows uses environment variables to locate system directories.

Example:

%windir%

This variable represents the **Windows installation directory**.

Example usage:

%windir%\\System32  
---

## **System32 Directory**

C:\\Windows\\System32

The **System32 folder** contains **critical operating system components**.

Examples include:

* System libraries

* Executable utilities

* Core Windows processes

---

## **Important Windows Directories Table**

| Directory | Purpose |
| ----- | ----- |
| **C:\\Windows** | Contains Windows operating system files |
| **C:\\Windows\\System32** | Critical system executables and libraries |
| **C:\\Users** | Stores user profiles |
| **C:\\Program Files** | Installed applications |
| **C:\\Program Files (x86)** | 32-bit programs on 64-bit systems |

---

### **Security Relevance**

Attackers often target:

* **System32**

* **Startup folders**

* **User profile directories**

These locations may contain **malware persistence mechanisms**.

---

### **Quick Summary**

* **C:\\Windows** contains the OS.

* **System32 contains critical system components**.

* Environment variables help locate system directories.

---

# **Windows User Accounts**

On a typical Windows system there are **two main account types**.

## **Administrator**

An **Administrator account** can:

* Add users

* Delete users

* Modify system settings

* Install programs

* Manage groups

---

## **Standard User**

A **Standard User** can:

* Modify **their own files**

* Use installed programs

* Cannot perform **system-level modifications**

---

### **Viewing Users and Groups**

To open Local User Management:

lusrmgr.msc

This opens **Local Users and Groups Management** where administrators can:

* Manage users

* Manage groups

* Modify permissions

---

### **Security Insight**

Cyber attackers often try to **escalate privileges** from:

Standard User → Administrator

This is called **Privilege Escalation**.

---

### **Quick Summary**

* Windows has **Administrator and Standard accounts**

* Admins control **system settings**

* Attackers frequently attempt **privilege escalation**

---

# **User Account Control (UAC)**

**User Account Control (UAC)** is a security feature that helps prevent **unauthorized system changes**.

UAC ensures that programs **cannot automatically run with administrative privileges**.

---

## **Why UAC Exists**

Without UAC:

* Malware running under an admin account

* Could **silently modify the entire system**

UAC adds a **security checkpoint**.

---

## **How UAC Works**

When a user performs an action requiring higher privileges:

1. Windows detects the privileged operation

2. The system prompts the user

3. The user must **confirm permission**

Example prompt:

Do you want to allow this app to make changes to your device?  
---

### **Important Detail**

Even when logged in as **Administrator**, the session runs with **limited privileges by default**.

Privileges are **temporarily elevated only when approved**.

---

### **FYI**

UAC **does not apply by default** to the built-in Administrator account.

---

### **Quick Summary**

* UAC protects against **malware executing with admin privileges**

* It **prompts users before system-level changes**

* It limits damage from compromised programs

---

# **Windows Administrative Tools**

## **Control Panel**

The **Control Panel** is used for **advanced system configuration**.

Examples of tasks:

* Network configuration

* User account settings

* System security

* Hardware configuration

---

### **Settings vs Control Panel**

| Tool | Purpose |
| ----- | ----- |
| **Settings** | Simplified configuration interface |
| **Control Panel** | Advanced system management |

---

## **Task Manager**

**Task Manager** shows system activity and running programs.

To open Task Manager:

Ctrl \+ Shift \+ Esc  
---

### **Information Provided**

Task Manager shows:

* Running applications

* Background processes

* CPU usage

* RAM usage

* Disk activity

* Network activity

---

### **Security Use Case**

Security analysts often use Task Manager to:

* Detect **suspicious processes**

* Identify **resource-heavy malware**

* Kill malicious programs

---

### **Quick Summary**

* **Control Panel** provides advanced system configuration.

* **Task Manager** monitors system processes and performance.

---

# **Commands & Tools Table**

| Command | Purpose |
| ----- | ----- |
| `lusrmgr.msc` | Opens Local Users and Groups manager |
| `Ctrl + Shift + Esc` | Opens Task Manager |
| `%windir%` | Points to the Windows directory |

---

# **Key Beginner Insights**

* **NTFS is the standard Windows file system**

* **System32 is one of the most critical OS directories**

* **Administrators have full system control**

* **UAC prevents unauthorized system changes**

* **Task Manager helps monitor system processes**

---

# **Personal Learning Notes**

Important lessons from studying Windows fundamentals:

* Understanding **file systems** helps detect hidden malware.

* Many attacks involve **privilege escalation**.

* Key system directories like **System32** often contain important forensic evidence.

* **UAC prompts should never be ignored**, especially in corporate environments.

Learning the **structure of Windows** is essential for:

* Malware analysis

* Incident response

* Penetration testing

* Digital forensics

---

# **Tips for Cybersecurity Students**

### **Tip 1**

Always check **running processes** when investigating a compromised system.

### **Tip 2**

Understand **user privilege levels** before attempting system modifications.

### **Tip 3**

Learn the **important Windows directories**, as malware frequently hides there.

---

# **Next Steps / Final Thoughts**

After learning these Windows fundamentals, the next recommended topics are:

* Windows **process management**

* Windows **registry analysis**

* Windows **event logs**

* Windows **privilege escalation techniques**

* Windows **Active Directory basics**

Platforms like **TryHackMe** provide labs that simulate **real attack scenarios**, making them ideal for practicing these concepts.

---

✅ **Final Takeaway**

Understanding **Windows internals** is critical for cybersecurity professionals.  
Mastering concepts like **file systems, permissions, system directories, and privilege management** builds the foundation for **defensive security and penetration testing**.

