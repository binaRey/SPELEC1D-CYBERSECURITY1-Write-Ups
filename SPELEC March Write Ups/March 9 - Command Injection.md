# **Command Injection (Remote Code Execution)**

## **Overview**

Command Injection is a web vulnerability that allows an attacker to execute operating system commands on a server through a vulnerable application. This occurs when user input is improperly sanitized and is directly passed into system-level commands.

Command Injection is also commonly referred to as:

* Remote Code Execution (RCE)  
* OS Command Injection

This vulnerability is considered extremely dangerous because successful exploitation may allow attackers to:

* Execute arbitrary system commands  
* Read sensitive files  
* Access user data  
* Enumerate the system  
* Gain remote shells  
* Escalate privileges  
* Completely compromise the server

Applications written in languages such as:

* PHP  
* Python  
* NodeJS  
* Java  
* ASP.NET

can all become vulnerable if they execute operating system commands using unsanitized user input.

---

# **How Command Injection Works**

## **Example Scenario**

Imagine a music search application where a user searches for a song title.

### **PHP Example Logic**

$title \= $\_GET\['title'\];  
system("grep $title songtitle.txt");  
---

# **Step-by-Step Breakdown**

## **Step 1 – User Input**

The user enters a song title into the application.

Example:

hello

The application stores this input inside:

$title  
---

## **Step 2 – Application Executes a System Command**

The application executes:

grep hello songtitle.txt

This searches for the song title inside a file.

---

## **Step 3 – Vulnerability Appears**

Because user input is directly inserted into the system command, an attacker can inject additional commands.

Example malicious input:

hello; whoami

The resulting command becomes:

grep hello; whoami songtitle.txt

Now the operating system executes:

whoami

This reveals the current system user running the application.

---

# **Why Command Injection Happens**

The vulnerability occurs because:

* User input is trusted  
* Input validation is missing  
* Dangerous functions execute OS commands directly  
* Special shell characters are not sanitized

Common dangerous functions include:

## **PHP**

system()  
exec()  
shell\_exec()  
passthru()

## **Python**

subprocess  
os.system

## **NodeJS**

exec()  
spawn()  
---

# **Python Example**

## **Flask Vulnerable Application**

The room also demonstrated a vulnerable Flask application.

### **Workflow**

1. Flask creates a web server  
2. User input is passed directly into a system command  
3. Visiting a URL executes the command

---

## **Example**

Visiting:

http://flaskapp.thm/whoami

executes:

whoami

on the server.

---

# **Risks of Command Injection**

## **High Impact Vulnerability**

Successful exploitation may lead to:

* Full server compromise  
* Data theft  
* Credential disclosure  
* Reverse shell access  
* Privilege escalation  
* Lateral movement  
* Malware deployment

---

# **Common Commands Attackers Execute**

## **Linux Commands**

whoami  
id  
hostname  
pwd  
ls  
cat /etc/passwd

## **Windows Commands**

whoami  
hostname  
dir  
type  
ipconfig  
---

# **Common Injection Operators**

Attackers often chain commands using shell operators.

## **Linux Operators**

;  
&&  
||  
|  
$  
\`\`

### **Examples**

hello; whoami  
hello && id  
hello | whoami  
---

# **Example Exploit**

## **Vulnerable Request**

GET /search.php?title=hello HTTP/1.1

## **Malicious Request**

GET /search.php?title=hello;whoami HTTP/1.1

The application unknowingly executes:

grep hello; whoami songtitle.txt  
---

# **Prevention Techniques**

## **Input Validation**

* Allow only expected characters  
* Reject shell metacharacters

Example:

\[a-zA-Z0-9\]  
---

## **Avoid System Commands**

Use safe programming functions instead of OS commands.

Example:

* Use database queries  
* Use filesystem APIs  
* Avoid shell execution entirely

---

## **Principle of Least Privilege**

Applications should run with minimal permissions.

If exploited, attacker impact becomes limited.

---

## **Escaping User Input**

Sanitize shell characters before execution.

Examples:

escapeshellarg()  
escapeshellcmd()  
---

# **Key Takeaways**

* Command Injection allows attackers to execute OS commands remotely.  
* It occurs when applications pass unsanitized input into system commands.  
* RCE vulnerabilities are among the most critical web vulnerabilities.  
* Even simple user input fields can become dangerous.  
* Proper validation and avoiding shell execution are the best defenses.

---

# **Answers**

## **What variable stores the user's input in the PHP code snippet in this task?**

$title  
---

## **What HTTP method is used to retrieve data submitted by a user in the PHP code snippet?**

GET  
---

## **If I wanted to execute the id command in the Python code snippet, what route would I need to visit?**

http://flaskapp.thm/id

# **Detecting Command Injection Vulnerabilities**

## **Overview**

Command Injection is a critical web vulnerability that allows attackers to execute operating system commands through a vulnerable application. This occurs when user input is passed directly into system commands without proper validation or sanitization. Because the application executes these commands using its own privileges, attackers may gain unauthorized access to sensitive files, system information, or even remote shell access.

This vulnerability is commonly associated with Remote Code Execution (RCE) because attackers can remotely run commands on the target machine. Applications written in languages such as PHP, Python, Node.js, and others may become vulnerable if they improperly handle user input.

Understanding how command injection behaves is important for identifying and securing vulnerable systems. Applications that process user-supplied input into operating system commands often reveal suspicious behaviour when tested with crafted payloads.

## **Key Takeaways**

* Command Injection occurs when user input is executed as part of a system command.  
* Attackers can manipulate shell operators such as:  
  * ;  
  * &  
  * &&  
* These operators allow multiple commands to run together.  
* Command Injection vulnerabilities may lead to:  
  * Remote Code Execution (RCE)  
  * Data theft  
  * Reverse shells  
  * Privilege escalation  
  * Server compromise  
* Two major detection methods exist:  
  * Blind Command Injection  
  * Verbose Command Injection

## **Blind Command Injection**

Blind Command Injection occurs when commands execute successfully, but the application does not display the output directly to the attacker.

### **Characteristics**

* No visible command output  
* Requires observing application behaviour  
* Often detected through delays or forced output

### **Common Detection Techniques**

#### **Time-Based Payloads**

Attackers use commands that intentionally delay the application response.

Examples:

Linux:

ping \-c 5 127.0.0.1  
sleep 5

Windows:

ping 127.0.0.1 \-n 5  
timeout 5

### **File Redirection Technique**

Attackers can redirect command output into a file and later read it.

Example:

whoami \> output.txt  
cat output.txt

### **Using curl for Testing**

The curl command can send crafted payloads to vulnerable applications.

Example:

curl http://vulnerable.app/process.php%3Fsearch%3DThe%20Beatles%3B%20whoami

This payload appends the whoami command to the original request.

## **Verbose Command Injection**

Verbose Command Injection provides direct feedback after executing commands.

### **Characteristics**

* Command output is displayed immediately  
* Easier to detect than blind injection  
* Useful for quick validation of vulnerabilities

### **Example**

If an attacker runs:

whoami

The application may directly display:

www-data

This reveals the user account running the application.

## **Useful Payloads**

### **Linux Payloads**

| Payload | Purpose |
| ----- | ----- |
| whoami | Displays the current user |
| ls | Lists directory contents |
| ping | Causes application delay |
| sleep | Delays execution |
| nc | Creates reverse shells |

### **Windows Payloads**

| Payload | Purpose |
| ----- | ----- |
| whoami | Displays current user |
| dir | Lists directory contents |
| ping | Creates delay |
| timeout | Delays execution |

## **Additional Information and Facts**

* Command Injection vulnerabilities often exist in:  
  * Search functionality  
  * File processing features  
  * System utilities integrated into web apps  
  * Admin panels  
* Input sanitization and parameterized execution help prevent attacks.  
* Least privilege principles reduce the impact of successful exploitation.  
* Reverse shells obtained through command injection can provide full server interaction.  
* Blind command injection requires patience and experimentation due to lack of direct feedback.  
* Linux and Windows command syntax differ significantly during exploitation.

## **Common Indicators of Vulnerability**

* Unusual application delays  
* Unexpected error messages  
* Commands executing after separators such as:  
  * ;  
  * &&  
  * |  
* File creation or modification on the server  
* Reflected command output

## **Security Recommendations**

* Never pass raw user input directly into system commands.  
* Use secure APIs instead of shell execution where possible.  
* Implement strict input validation and sanitization.  
* Disable unnecessary system command execution.  
* Apply proper access controls and least privilege permissions.  
* Monitor logs for suspicious command patterns.

---

What payload would I use if I wanted to determine what user the application is running as?  
 Answer: whoami

What popular network tool would I use to test for blind command injection on a Linux machine?  
 Answer: ping

What payload would I use to test a Windows machine for blind command injection?  
 Answer: timeout

# **Preventing Command Injection Vulnerabilities**

## **Overview**

Command Injection vulnerabilities occur when an application improperly handles user input and executes it as part of a system command. Because these commands are executed directly on the operating system, attackers may gain unauthorized access, execute malicious code, or compromise the entire server.

Preventing command injection is an essential part of secure application development. Developers must ensure that applications never trust user input directly and must carefully control how data is processed before interacting with the operating system.

This section focuses on defensive techniques such as input sanitization, filtering, validation, and avoiding dangerous functions that execute system commands.

## **Key Takeaways**

* Command Injection can often be prevented through proper input validation and sanitization.  
* Dangerous system execution functions should be avoided whenever possible.  
* Applications should only allow expected input formats.  
* Filtering special characters reduces the risk of command chaining.  
* Security controls should never rely solely on client-side validation.  
* Different programming languages contain functions capable of executing operating system commands.

## **Vulnerable Functions**

Some programming languages provide built-in functions that directly interact with the operating system shell. If developers pass unsanitized user input into these functions, attackers may execute arbitrary commands.

### **Common Vulnerable PHP Functions**

| Function | Purpose |
| ----- | ----- |
| exec() | Executes a system command |
| passthru() | Executes commands and displays raw output |
| system() | Executes commands and returns output |

These functions become dangerous when user-controlled data is included without proper validation.

## **Example of Safe Input Validation**

A secure application should only accept specific data formats.

### **Example Scenario**

An application expects only numerical input.

Security controls:

* Accept digits only (0-9)  
* Reject letters and special characters  
* Prevent shell operators from executing

This prevents payloads such as:

whoami

or:

cat /etc/passwd

from being processed.

## **Input Sanitization**

Input sanitization refers to the process of cleaning and validating user input before it is used by the application.

### **Common Sanitization Techniques**

* Removing dangerous characters:  
  * ;  
  * &  
  * |  
  * \>  
  * \<  
  * /  
* Allowing only whitelisted characters  
* Restricting inputs to expected formats  
* Rejecting invalid requests automatically

### **Example**

A field designed for age input should only allow:

0-9

Anything else should be rejected.

## **Filtering User Input**

Applications often use filtering functions to validate user data before processing it.

### **Example in PHP**

The filter\_input() function can:

* Validate numbers  
* Validate emails  
* Reject malicious input  
* Reduce injection risks

This prevents attackers from inserting commands into application parameters.

## **Bypassing Filters**

Attackers frequently attempt to bypass weak filtering systems.

### **Common Bypass Techniques**

* Using hexadecimal encoding  
* URL encoding  
* Alternative shell syntax  
* Character substitutions  
* Nested commands

### **Example**

If quotation marks are filtered:

"

An attacker may instead use:

\\x22

Even though the format differs, the operating system may interpret it the same way.

## **Additional Information and Facts**

* Input sanitization alone is not always enough.  
* Whitelisting is more secure than blacklisting.  
* Server-side validation is mandatory.  
* Least privilege permissions help reduce damage during exploitation.  
* Attackers often chain commands using:  
  * ;  
  * &&  
  * |  
* Reverse shells are a common post-exploitation technique after successful command injection.

## **Best Security Practices**

### **Developers Should:**

* Avoid unnecessary system command execution  
* Use safe APIs instead of shell commands  
* Validate all user input  
* Implement allow-list filtering  
* Escape shell arguments properly  
* Apply least privilege permissions  
* Log suspicious input attempts  
* Perform regular security testing

## **Common Signs of Command Injection**

* Unexpected application delays  
* Strange server behaviour  
* Unusual file creation  
* System command output appearing on pages  
* Unexpected network traffic  
* High CPU or memory usage

## **Impact of Command Injection**

Successful exploitation may lead to:

* Remote Code Execution (RCE)  
* File disclosure  
* Data theft  
* Server takeover  
* Privilege escalation  
* Reverse shell access  
* Lateral movement inside networks

---

What is the term for the process of "cleaning" user input that is provided to an application?  
 Answer: Sanitisation

What user is this application running as?  
 Answer: www-data

What are the contents of the flag located in /home/tryhackme/flag.txt?  
 Answer: THM{c0mmand\_1nj3ct10n\_succ3ss}

