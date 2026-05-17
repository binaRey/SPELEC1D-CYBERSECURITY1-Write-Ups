# **Windows Fundamentals Pt.3 — TryHackMe Write-Up**

**Author:** Reynan S. Cortado  
**Platform:** TryHackMe  
**Topic:** Windows & Active Directory Fundamentals

---

# **Introduction**

This module explores **built-in Windows security tools and update mechanisms** that help protect systems from threats. Understanding these features is critical for **cybersecurity professionals, system administrators, and IT support specialists**.

The topics covered include:

* **Windows Updates**

* **Windows Security Center**

* **Firewall protection**

* **BitLocker encryption**

* **Trusted Platform Module (TPM)**

* **Volume Shadow Copy Service (VSS)**

These tools help maintain **system integrity, data protection, and security monitoring**.

---

# **Tools & Operating Systems**

## **Operating System**

* Microsoft Windows

## **Windows Security Tools Covered**

| Tool | Purpose |
| ----- | ----- |
| **Windows Update** | Installs security patches and system updates |
| **Windows Security** | Central dashboard for device protection |
| **Windows Defender Firewall** | Controls incoming and outgoing network traffic |
| **BitLocker** | Full disk encryption feature |
| **Trusted Platform Module (TPM)** | Hardware security component |
| **Volume Shadow Copy Service (VSS)** | Backup and restore mechanism |

---

### **Quick Summary**

* Windows provides **built-in security protections**.

* Updates and security tools help **prevent malware infections**.

* Encryption and backup services protect **sensitive data**.

---

# **Windows Updates**

**Windows Update** is a service provided by Microsoft that delivers:

* **Security patches**

* **Feature updates**

* **Bug fixes**

* **Microsoft Defender updates**

These updates help keep the operating system **secure and stable**.

---

## **Important Behavior in Modern Windows**

Older Windows versions allowed users to **ignore updates indefinitely**.

In newer versions like **Windows 10**, updates:

* Cannot be permanently disabled

* Can only be **postponed**

* Eventually **install automatically**

This ensures systems remain **secure against newly discovered vulnerabilities**.

---

### **FYI**

Most Windows updates require a **system reboot** to complete installation.

---

### **Tip**

Always install **security updates immediately**, as attackers often exploit **unpatched vulnerabilities**.

---

### **Quick Summary**

* Windows Update delivers **security patches and system improvements**.

* Updates are essential for **protecting against vulnerabilities**.

* Modern Windows versions **force updates eventually**.

---

# **Windows Security**

**Windows Security** acts as a centralized dashboard for monitoring the device’s protection status.

It allows users to manage:

* Virus protection

* Firewall settings

* Device security

* App protection

---

## **Status Indicators**

| Color | Meaning |
| ----- | ----- |
| **Green** | System is secure |
| **Yellow** | Security recommendation available |
| **Red** | Immediate attention required |

---

## **Virus & Threat Protection**

This feature helps detect and remove malware.

### **Scan Types**

* **Quick Scan** – scans common malware locations

* **Full Scan** – scans the entire system

* **Custom Scan** – scans specific files or folders

---

## **Security Settings**

### **Real-Time Protection**

* Detects malware **as it attempts to run or install**

### **Cloud-Delivered Protection**

* Uses **cloud intelligence** for faster detection

### **Automatic Sample Submission**

* Sends suspicious files to Microsoft for analysis

### **Controlled Folder Access**

* Protects files from **unauthorized changes**

### **Exclusions**

* Files or folders excluded from antivirus scanning

---

### **Tip**

Security analysts should **review threat history** regularly to check:

* Quarantined malware

* Allowed threats

* Recent scans

---

### **Quick Summary**

* Windows Security manages **device protection features**.

* Antivirus scanning protects against malware.

* Real-time protection helps **stop threats immediately**.

---

# **Firewall & Network Protection**

A **firewall** monitors and controls incoming and outgoing network traffic.

It helps prevent **unauthorized access** to a system.

Command to open Windows Firewall:

WF.msc  
---

## **Firewall Profiles**

| Profile | Purpose |
| ----- | ----- |
| **Domain** | Used when connected to a corporate network |
| **Private** | Used for trusted networks such as home Wi-Fi |
| **Public** | Used for untrusted networks like airports |

---

### **Example Scenario**

If connected to **airport Wi-Fi**, the firewall profile should be:

Public Network

This profile has the **strictest security settings**.

---

### **Tip**

Always use the **Public profile** when connecting to unfamiliar networks.

---

### **Quick Summary**

* Firewalls protect systems from **unauthorized network access**.

* Windows uses **three network profiles**.

* Public networks require **strict security controls**.

---

# **App & Browser Control**

Windows includes **Microsoft Defender SmartScreen**, which protects users from:

* Phishing websites

* Malicious downloads

* Unsafe applications

SmartScreen analyzes files and websites to determine **potential risks**.

---

## **Exploit Protection**

Exploit protection helps defend against:

* Memory corruption attacks

* Application vulnerabilities

* Code injection attacks

This feature is built directly into Windows.

---

### **Quick Summary**

* SmartScreen protects against **malicious websites and downloads**.

* Exploit protection prevents **common attack techniques**.

---

# **Device Security**

Device Security includes **hardware-based protection features**.

---

## **Core Isolation**

Core Isolation uses **virtualization-based security** to protect sensitive parts of the operating system.

### **Memory Integrity**

This feature prevents attackers from inserting **malicious code into protected processes**.

---

## **Trusted Platform Module (TPM)**

**TPM (Trusted Platform Module)** is a hardware-based security chip that performs cryptographic operations.

Functions include:

* Secure encryption key storage

* System integrity verification

* Support for disk encryption

TPM is designed to be **tamper-resistant**.

---

### **Tip**

Many enterprise security features rely on **TPM hardware support**.

---

### **Quick Summary**

* Device Security protects **core system components**.

* TPM provides **hardware-level security**.

* Memory integrity protects against **kernel-level attacks**.

---

# **BitLocker Encryption**

**BitLocker Drive Encryption** protects data stored on a computer’s drive.

It prevents attackers from accessing data if a device is:

* Lost

* Stolen

* Improperly disposed

---

## **How BitLocker Works**

BitLocker encrypts the entire disk and integrates with **TPM hardware**.

When the system boots:

* TPM verifies system integrity

* If everything is valid, the drive unlocks automatically

---

### **Systems Without TPM**

If a computer does not have **TPM version 1.2 or later**, the user must provide:

USB startup key

This key unlocks the encrypted drive.

---

### **Security Benefit**

BitLocker prevents attackers from accessing files even if they:

* Remove the hard drive

* Boot from another operating system

---

### **Quick Summary**

* BitLocker encrypts the **entire disk**.

* Works with **TPM for secure authentication**.

* Protects data from **physical theft**.

---

# **Volume Shadow Copy Service (VSS)**

The **Volume Shadow Copy Service (VSS)** creates **snapshots of system data**.

These snapshots allow Windows to restore files or the system to a previous state.

---

## **Functions of VSS**

If VSS is enabled, users can:

* Create **restore points**

* Perform **system restore**

* Configure restore settings

* Delete restore points

---

### **Security Insight**

Backup services like VSS are important for:

* **Disaster recovery**

* **Ransomware recovery**

* **System rollback**

---

### **Warning**

Some ransomware attacks attempt to **delete VSS backups** to prevent recovery.

---

### **Quick Summary**

* VSS creates **system snapshots**.

* Used for **system restore and backups**.

* Critical for **disaster recovery**.

---

# **Windows Command Reference Table**

| Command | Purpose |
| ----- | ----- |
| `WF.msc` | Opens Windows Defender Firewall |
| `msinfo32.exe` | Opens System Information |
| `compmgmt.msc` | Opens Computer Management |
| `resmon.exe` | Opens Resource Monitor |
| `regedt32.exe` | Opens Registry Editor |

---

# **Important Windows Security Components**

| Component | Purpose |
| ----- | ----- |
| **Windows Security** | Central protection dashboard |
| **Windows Defender** | Built-in antivirus |
| **Firewall** | Controls network access |
| **BitLocker** | Disk encryption |
| **TPM** | Hardware cryptographic security |
| **VSS** | Backup and system restore service |

---

# **Key Beginner Insights**

Important takeaways from this module:

* **Windows Updates protect systems from vulnerabilities**

* **Windows Security provides centralized protection tools**

* **Firewalls control network access**

* **BitLocker encrypts data to prevent unauthorized access**

* **TPM provides hardware-based cryptographic security**

* **VSS enables system restore and backups**

---

# **Personal Learning Notes**

Studying Windows security features highlights how modern operating systems provide **multiple layers of protection**.

Key lessons learned:

* Security updates are essential for protecting systems.

* Built-in antivirus and firewall tools provide **baseline protection**.

* Encryption technologies like **BitLocker** protect sensitive data.

* Backup services like **VSS** are critical for **recovery after system failures or attacks**.

Understanding these tools helps cybersecurity professionals detect, investigate, and respond to **security incidents**.

---

# **Next Steps / Final Thoughts**

After completing **Windows Fundamentals Part 3**, recommended next topics include:

* Windows **Event Log analysis**

* Windows **process investigation**

* Windows **privilege escalation techniques**

* Windows **Active Directory security**

* Windows **malware persistence mechanisms**

Hands-on practice through **TryHackMe** labs is highly recommended to reinforce these concepts.

---

✅ **Final Takeaway**

Understanding **Windows security tools such as Windows Update, Windows Security, BitLocker, TPM, and VSS** provides the foundation for **defending and investigating Windows systems in cybersecurity environments**.

