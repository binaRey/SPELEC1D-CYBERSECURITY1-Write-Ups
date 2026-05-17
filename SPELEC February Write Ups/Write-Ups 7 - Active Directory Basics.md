# **Active Directory Basics — TryHackMe Write-Up** 

**Author:** Reynan S. Cortado  
**Platform:** TryHackMe  
**Topic:** Windows & Active Directory Fundamentals

---

# **Introduction**

This guide explains the **core concepts of Windows Active Directory (AD)** and how organizations manage users, computers, authentication, and policies across a network.

In modern enterprise environments, manually managing hundreds or thousands of computers is impossible. **Active Directory solves this problem by centralizing identity management, authentication, and policy enforcement.**

In this module you will learn:

* How **Windows Domains** work

* The role of **Domain Controllers**

* How **Active Directory objects** (users, groups, machines) are structured

* How administrators manage accounts and computers

* Authentication mechanisms like **Kerberos** and **NTLM**

* Advanced AD concepts like **Trees, Forests, and Trusts**

This guide expands the original lab notes and adds **extra cybersecurity insights useful for penetration testing and blue-team security analysis.**

---

# **Tools & Operating Systems**

## **Tools Used**

* **Remmina**

* **Command Prompt (cmd)**

* **PowerShell**

* **Active Directory Users and Computers**

## **Operating Systems**

* **Windows Server**

* **Kali Linux**

### **Why These Tools Matter**

* **Remmina** → Used to remotely access Windows systems via RDP

* **PowerShell** → Powerful automation tool for managing AD objects

* **Windows Server** → Runs **Active Directory Domain Services (AD DS)**

---

### **Quick Summary**

* Active Directory environments typically run on **Windows Server**

* Administrators manage AD using **PowerShell and GUI tools**

* Security professionals often access labs via **RDP clients like Remmina**

---

# **Windows Domains**

## **What is a Windows Domain?**

A **Windows Domain** is a network where **users, computers, and resources are centrally managed** through **Active Directory**.

Instead of managing each computer individually:

❌ Without Domain

* Every PC has its own accounts

* Manual configuration required

✅ With Domain

* One centralized database

* Accounts usable on any machine

---

## **Active Directory (AD)**

**Active Directory** is a **directory database service** that stores information about objects in the network.

Examples of stored data:

* usernames

* passwords

* phone numbers

* email addresses

* group memberships

* machine accounts

* permissions

The system running Active Directory is called a **Domain Controller (DC).**

---

## **Domain Controller**

A **Domain Controller (DC)** is a server responsible for:

* Authentication

* Authorization

* Directory management

* Security policies

Every login request in the domain is verified by a **Domain Controller**.

---

## **Real-World Example**

In **schools or universities**, students receive a login such as:

student01  
password123

That login works on **any computer in the campus network** because authentication is checked against **Active Directory**, not the local machine.

Administrators can also enforce policies such as:

* Blocking Control Panel access

* Preventing software installation

* Enforcing password rules

---

### **Quick Summary**

* **Active Directory \= central database**

* **Domain Controller \= server running AD**

* Domains allow **centralized authentication and policy control**

* Users can log into **any domain-joined computer**

---

# **Remote Access via RDP**

Administrators often manage servers remotely using **Remote Desktop Protocol (RDP).**

Example lab access used:

Username: Administrator  
Password: Password321

Connection via Remmina.

---

## **RDP Command Example (Linux)**

remmina

Or using an RDP client:

xfreerdp /u:Administrator /p:Password321 /v:10.10.x.x  
---

### **Why RDP is Important in Cybersecurity**

Penetration testers frequently find:

* exposed RDP services

* weak credentials

* lateral movement opportunities

RDP is a **common attack vector** in enterprise breaches.

---

### **Quick Summary**

* **RDP allows remote Windows access**

* Used heavily in **administration and labs**

* Often targeted during **network intrusions**

---

# **Active Directory Objects**

Active Directory stores **objects** representing resources on the network.

Common objects include:

* Users

* Computers

* Groups

* Printers

* Shared folders

---

## **Users**

A **User Object** represents a person or service account.

Users are called **Security Principals**, meaning they can:

* authenticate

* access resources

* receive permissions

Two types of users exist:

### **Human Users**

Example:

john.doe  
mary.smith

### **Service Accounts**

Used by services like:

* databases

* web servers

* background processes

Example:

IIS\_service  
SQL\_service  
---

## **Machine Accounts**

Every computer joining the domain gets a **machine account**.

Naming format:

COMPUTERNAME$

Example:

TOM-PC$

Important facts:

* Machine accounts are also **security principals**

* Passwords automatically rotate

* Typically **120 random characters**

---

## **Security Groups**

Groups allow administrators to **assign permissions to many users at once.**

Example groups:

| Group | Purpose |
| ----- | ----- |
| Domain Admins | Full domain control |
| Domain Users | All normal users |
| Domain Computers | All computers |
| Backup Operators | Can access all files |

---

### **Quick Summary**

* **Users** represent people or services

* **Machines** represent computers

* **Groups** manage permissions efficiently

* All of these are **AD objects**

---

# **Organizational Units (OUs)**

An **Organizational Unit (OU)** is a container used to organize users and computers.

Example company structure:

THM  
├── IT  
├── Management  
├── Marketing  
└── Sales

OUs are useful for:

* applying policies

* organizing departments

* delegating administration

Important rule:

⚠️ A user can belong to **only one OU**.

---

## **Default Containers in AD**

| Container | Purpose |
| ----- | ----- |
| Builtin | Default Windows groups |
| Computers | Newly joined machines |
| Users | Default domain users |
| Domain Controllers | Domain controller servers |

---

## **OUs vs Security Groups**

| Feature | OU | Security Group |
| ----- | ----- | ----- |
| Main purpose | Apply policies | Assign permissions |
| Membership | One OU per user | Multiple groups allowed |
| Used for | Management | Resource access |

---

### **Quick Summary**

* **OUs organize AD objects**

* Used for **policy application**

* **Groups control permissions**

---

# **Managing Users in Active Directory**

Administrators can manage accounts using **PowerShell**.

Example: Reset user password

Set-ADAccountPassword sophie \-Reset \-NewPassword (Read-Host \-AsSecureString \-Prompt 'New Password') \-Verbose

Example password used:

abcD12345\*  
---

## **Force Password Change**

To require a user to change password at next login:

Set-ADUser \-ChangePasswordAtLogon $true \-Identity sophie \-Verbose  
---

## **Delegation**

Delegation allows non-admin users to perform limited administrative tasks.

Example:

Helpdesk staff can reset passwords without being Domain Admins.

This reduces security risk.

---

### **Quick Summary**

* PowerShell can manage AD users

* Password resets are common helpdesk tasks

* **Delegation allows controlled privilege distribution**

---

# **Managing Computers in AD**

Organizations typically separate computers into OUs.

Example structure:

Computers  
├── Servers  
└── Workstations

This separation is recommended.

Reason:

Servers require **different security policies** than workstations.

---

### **Quick Summary**

* Computers should be grouped logically

* Separate **servers and workstations**

* Makes policy management easier

---

# **Group Policy Objects (GPO)**

**Group Policy Objects (GPOs)** allow administrators to enforce configuration across the domain.

Examples:

* password complexity

* firewall rules

* software restrictions

* desktop settings

---

## **GPO Distribution**

Policies are distributed through a network share called:

SYSVOL

Location example:

\\\\domain.local\\SYSVOL  
---

### **Quick Summary**

* GPOs enforce **security policies**

* Distributed via **SYSVOL**

* Can apply to **users and computers**

---

# **Authentication Methods**

Windows domains support two main authentication protocols.

---

## **Kerberos (Default)**

Kerberos is the **modern authentication protocol** used in Windows.

Authentication process:

1. User requests **Ticket Granting Ticket (TGT)**

2. KDC verifies credentials

3. User receives TGT

4. User requests **Service Ticket (TGS)**

5. Access granted to service

Key component:

**KDC — Key Distribution Center**

---

## **NetNTLM**

Legacy authentication protocol.

Uses **challenge-response authentication**.

Process:

1. Client sends username

2. Server sends challenge

3. Client generates response using NTLM hash

4. Domain controller verifies response

Important security fact:

⚠️ The **password itself is never transmitted**.

---

### **Quick Summary**

* **Kerberos \= default modern authentication**

* **NTLM \= legacy protocol**

* Authentication handled by **Domain Controller**

---

# **Trees, Forests, and Trusts**

Large organizations may have **multiple domains**.

---

## **Domain Tree**

A **tree** is a group of domains sharing the same namespace.

Example:

thm.local  
├── uk.thm.local  
└── us.thm.local  
---

## **Forest**

A **forest** is a group of domain trees with different namespaces.

Example:

thm.local  
mht.local  
corp.internal  
---

## **Trust Relationships**

Trusts allow domains to share resources.

Types:

### **One-Way Trust**

Domain A trusts Domain B.

Users in B can access A.

---

### **Two-Way Trust**

Both domains trust each other.

Common in forests.

---

### **Quick Summary**

* **Tree \= domains sharing namespace**

* **Forest \= multiple domain trees**

* **Trusts allow cross-domain access**

---

# **Commands & Flags Table**

| Command | Flags | Description |
| ----- | ----- | ----- |
| Set-ADAccountPassword | \-Reset | Reset user password |
|  | \-NewPassword | Set new password |
| Set-ADUser | \-ChangePasswordAtLogon | Force password change |
| Read-Host | \-AsSecureString | Secure password input |

---

# **Important AD Components**

| Component | Purpose |
| ----- | ----- |
| Active Directory | Centralized identity database |
| Domain Controller | Server that manages authentication |
| OU | Organizational structure |
| Security Group | Permission management |
| SYSVOL | GPO distribution |

---

# **Key Beginner Insights**

Important lessons from this module:

* **Active Directory is the backbone of enterprise Windows networks**

* Most cyber attacks target **AD misconfigurations**

* Domain Controllers are **high-value targets**

* Kerberos tickets can be abused in attacks like:

  * **Kerberoasting**

  * **Golden Ticket**

---

# **Personal Learning Notes**

Key things learned during the lab:

* Understanding how **domains centralize authentication**

* Learning how **PowerShell manages AD accounts**

* Seeing how **organizational structures mirror business departments**

* Practicing **delegation of administrative privileges**

---

# **Tips for Cybersecurity Students**

💡 **Tip**

Learn these AD attacks next:

* Pass-the-Hash

* Kerberoasting

* DCSync

* Golden Ticket

Most enterprise breaches involve **Active Directory compromise**.

---

# **Next Steps / Recommended Topics**

After mastering these basics, study:

* Active Directory Enumeration

* BloodHound

* Privilege Escalation in AD

* Kerberos attacks

* Group Policy abuse

Good platforms to practice:

* TryHackMe

* Hack The Box

---

✅ **Final Takeaway**

Active Directory is **one of the most important technologies in cybersecurity**.

Understanding how it works is essential for:

* **System administrators**

* **Security engineers**

* **Penetration testers**

Mastering AD concepts will dramatically improve your ability to **defend or attack enterprise networks.**

