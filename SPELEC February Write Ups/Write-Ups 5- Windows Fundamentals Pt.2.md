# **Windows Fundamentals Pt.2 — TryHackMe Write-Up**

**Author:** Reynan S. Cortado  
**Platform:** TryHackMe  
**Topic:** Windows & Active Directory Fundamentals

---

# **Introduction**

This guide covers **system configuration and monitoring tools in Windows** that are frequently used by **system administrators, cybersecurity analysts, and penetration testers**.

The module explores:

* **Computer Management utilities**

* **System Information tools**

* **Resource monitoring**

* **Command-line usage**

* **Windows Registry configuration**

Understanding these tools is essential for:

* **System diagnostics**

* **Security investigations**

* **Malware analysis**

* **Incident response**

---

# **Tools & Operating Systems**

## **Operating System**

* Microsoft Windows

## **Windows Tools Covered**

| Tool | Purpose |
| ----- | ----- |
| **Computer Management** | Central administrative console |
| **System Information** | Displays hardware and software configuration |
| **Resource Monitor** | Monitors CPU, memory, disk, and network usage |
| **Command Prompt** | Command-line interface for system interaction |
| **Registry Editor** | Manage Windows system configuration database |

---

### **Quick Summary**

* Windows includes several **built-in administrative tools**.

* These tools are essential for **system monitoring and troubleshooting**.

* Cybersecurity analysts frequently use them during **incident investigations**.

---

# **Computer Management**

The **Computer Management utility** is an administrative console that combines several Windows management tools in one interface.

Command to open it:

compmgmt.msc

The tool contains **three main sections**:

* **System Tools**

* **Storage**

* **Services & Applications**

---

## **System Tools**

### **Task Scheduler**

Task Scheduler allows users to **automate tasks**.

Tasks can:

* Run programs or scripts

* Execute at login or logout

* Run on a scheduled interval

Example schedule:

* Every **5 minutes**

* At **system startup**

* At a **specific time daily**

---

### **Event Viewer**

**Event Viewer** records system activity logs.

These logs help administrators:

* Diagnose system issues

* Investigate suspicious activity

* Review system errors

Logs can include:

* Security events

* System errors

* Application events

---

## **Storage**

### **Disk Management**

Disk Management allows administrators to perform advanced storage operations.

Common tasks include:

* Setting up a **new drive**

* **Extending partitions**

* **Shrinking partitions**

* Changing **drive letters**

Example drive letters:

C:  
D:  
E:  
---

## **Services & Applications**

### **Windows Management Instrumentation (WMI)**

**WMI** allows administrators to manage systems through **scripts or automation tools**.

Languages commonly used with WMI include:

* PowerShell

* VBScript

Windows also provides a command-line interface:

WMIC

This tool allows system management through **scripts and command-line automation**.

---

### **Tip**

Automated tasks in **Task Scheduler** are sometimes used by **malware for persistence**, making it an important place to investigate during security analysis.

---

### **Quick Summary**

* **Computer Management** centralizes multiple administrative tools.

* **Task Scheduler** automates system tasks.

* **Event Viewer** logs system events.

* **Disk Management** manages storage.

* **WMI** enables system automation and remote management.

---

# **System Information**

The **System Information tool** provides a detailed overview of the system’s hardware and software configuration.

Command to open:

msinfo32.exe

This tool gathers information about:

* Hardware components

* System resources

* Software environment

---

## **System Summary**

Displays general system information such as:

* Processor brand and model

* System manufacturer

* Operating system version

* Installed memory

---

## **Components Section**

Displays detailed information about installed hardware such as:

* Display devices

* Input devices

* Network adapters

Some sections may appear empty depending on the hardware installed.

---

## **Software Environment**

This section contains information about:

* Installed software

* System drivers

* Network connections

* Environment variables

Example environment variable:

ComSpec \= %SystemRoot%\\system32\\cmd.exe  
---

### **Quick Summary**

* **msinfo32** shows detailed system information.

* Includes hardware, drivers, and software environment.

* Useful for **system diagnostics and troubleshooting**.

---

# **Resource Monitor**

The **Resource Monitor** tool displays system resource usage in real time.

Command to open:

resmon.exe  
---

## **Information Provided**

Resource Monitor tracks:

* **CPU usage**

* **Memory usage**

* **Disk activity**

* **Network activity**

It can also show:

* Which processes are accessing files

* Which processes are using network connections

---

### **Example Capabilities**

Users can:

* Filter data by process

* Stop or start services

* Terminate unresponsive programs

---

### **Tip**

Resource Monitor is extremely useful for identifying:

* **Malicious processes**

* **High CPU usage malware**

* **Unusual network activity**

---

### **Quick Summary**

* **resmon.exe** monitors system performance.

* Shows real-time usage of system resources.

* Useful for **performance analysis and security monitoring**.

---

# **Command Prompt**

The **Command Prompt** is the traditional command-line interface used to interact with Windows.

Command to open:

cmd.exe

Before graphical interfaces were introduced, this was the **primary way to interact with the operating system**.

Even today, command-line tools are still used for:

* Troubleshooting

* Network diagnostics

* System configuration

---

## **Example Command**

### **Network Configuration**

ipconfig

Displays basic network configuration information.

To show **detailed network information**:

ipconfig /all  
---

## **System Configuration Example**

Example command used in system configuration:

C:\\Windows\\System32\\cmd.exe /k %windir%\\system32\\ipconfig.exe

This launches **Command Prompt** and automatically runs `ipconfig`.

---

### **FYI**

Even though Windows uses a **GUI**, many advanced troubleshooting tasks are still performed through **command-line tools**.

---

### **Quick Summary**

* `cmd.exe` launches **Command Prompt**.

* Command-line tools allow **advanced system diagnostics**.

* `ipconfig /all` shows detailed network configuration.

---

# **Windows Registry**

The **Windows Registry** is a hierarchical database that stores configuration information for:

* Operating system settings

* Hardware devices

* Installed applications

* User preferences

---

## **Registry Editor**

The registry can be accessed through **Registry Editor**.

Command to open:

regedt32.exe  
---

## **Why the Registry is Important**

Windows constantly references the registry to:

* Load system settings

* Configure hardware devices

* Manage installed programs

---

### **Security Insight**

Attackers often modify the registry to:

* Achieve **persistence**

* Automatically run malware on startup

* Modify system behavior

---

### **Warning**

⚠ **Editing the registry incorrectly can break the operating system.**

Always back up registry keys before modifying them.

---

### **Quick Summary**

* The registry stores **critical system configuration data**.

* Accessed through **Registry Editor**.

* Often targeted by **malware for persistence**.

---

# **Windows Command Reference Table**

| Command | Description |
| ----- | ----- |
| `compmgmt.msc` | Opens Computer Management |
| `msinfo32.exe` | Opens System Information |
| `resmon.exe` | Opens Resource Monitor |
| `cmd.exe` | Launches Command Prompt |
| `ipconfig` | Displays network configuration |
| `ipconfig /all` | Displays detailed network configuration |
| `regedt32.exe` | Opens Registry Editor |
| `control.exe` | Opens Control Panel |
| `UserAccountControlSettings.exe` | Opens UAC settings |

---

# **Important Windows Directories**

| Directory | Purpose |
| ----- | ----- |
| **C:\\Windows** | Core Windows operating system files |
| **C:\\Windows\\System32** | Critical system executables |
| **C:\\Users** | User profile directories |
| **C:\\Program Files** | Installed applications |
| **C:\\Temp / Temp folders** | Temporary files |

---

### **Cybersecurity Relevance**

These directories are frequently inspected during:

* Malware analysis

* Incident response

* Digital forensics investigations

---

# **Key Beginner Insights**

Important takeaways from this module:

* **Computer Management centralizes administrative tools**

* **Event Viewer logs system activity**

* **Resource Monitor tracks system performance**

* **Command Prompt remains essential for troubleshooting**

* **Registry stores critical configuration settings**

---

# **Personal Learning Notes**

Studying these Windows tools highlights how much **system activity is visible through built-in utilities**.

Key lessons learned:

* Many system tools can be accessed using **simple command shortcuts**.

* Monitoring tools like **Resource Monitor** help detect suspicious processes.

* The **Windows Registry** is a critical component of the operating system.

* Administrative tools provide powerful insight into **system configuration and behavior**.

Understanding these utilities is essential for:

* **Cybersecurity analysis**

* **System administration**

* **Digital forensics**

* **Penetration testing**

---

# **Next Steps / Final Thoughts**

After completing this module, recommended next learning topics include:

* Windows **event log analysis**

* Windows **process management**

* Windows **privilege escalation techniques**

* Windows **Active Directory fundamentals**

* Windows **malware persistence mechanisms**

Hands-on practice through platforms like **TryHackMe** helps reinforce these concepts through **interactive labs and simulated environments**.

---

✅ **Final Takeaway**

Mastering Windows administrative tools such as **Computer Management, System Information, Resource Monitor, Command Prompt, and Registry Editor** is a fundamental step toward becoming proficient in **cybersecurity and system administration**.

