# **Server-Side Request Forgery (SSRF)**

## **Overview**

Server-Side Request Forgery (SSRF) is a web vulnerability that allows attackers to force a web server to make HTTP requests on their behalf to internal or external resources.

Instead of directly accessing restricted resources, attackers abuse the vulnerable server to perform requests for them. Since the requests originate from the trusted server itself, attackers may gain access to:

* Internal services  
* Restricted endpoints  
* Cloud metadata  
* Sensitive files  
* Authentication tokens

SSRF vulnerabilities are particularly dangerous in cloud and internal network environments because they can expose systems that are not publicly accessible.

## **Key Takeaways**

* SSRF allows attackers to force servers to make arbitrary HTTP requests.  
* There are two types of SSRF:  
  * Regular SSRF  
  * Blind SSRF  
* SSRF can expose:  
  * Internal services  
  * Sensitive credentials  
  * Authentication tokens  
  * Cloud metadata  
* Developers commonly attempt SSRF protection using:  
  * Deny Lists  
  * Allow Lists  
* Attackers can bypass weak filtering using:  
  * Alternate localhost references  
  * Open Redirects  
  * Directory traversal tricks

## **What is SSRF?**

### **Definition**

SSRF stands for:

Server-Side Request Forgery

The vulnerability occurs when user input controls server-side requests.

### **Example Scenario**

A web application fetches an image from a URL supplied by the user:

https://website.thm/fetch?url=http://example.com/image.png

An attacker may replace the URL with an internal resource:

http://127.0.0.1/admin

The server now requests internal content on behalf of the attacker.

## **Types of SSRF**

### **Regular SSRF**

* The response is returned directly to the attacker.  
* Easier to exploit and verify.

### **Blind SSRF**

* The request occurs server-side.  
* No output is returned to the attacker.  
* Requires external monitoring tools such as:  
  * RequestBin  
  * Burp Collaborator  
  * Custom HTTP servers

## **Common SSRF Entry Points**

### **Full URL Parameters**

?url=http://example.com

### **Hidden Form Fields**

\<input type="hidden" name="api" value="http://api.site.thm"\>

### **Hostname Parameters**

?srv=filestorage.cloud.thm

### **Path-Based Parameters**

?path=/images/avatar.png

## **SSRF Impact**

Successful SSRF exploitation may lead to:

* Internal network scanning  
* Unauthorized data access  
* Cloud metadata theft  
* Credential exposure  
* Access to admin panels  
* Remote service interaction

## **Deny Lists**

### **What is a Deny List?**

A Deny List blocks:

* Specific domains  
* IP addresses  
* Keywords

### **Common Blocked Targets**

localhost  
127.0.0.1  
169.254.169.254

### **Bypass Techniques**

Attackers may use alternative localhost representations:

0  
0.0.0.0  
127.1  
2130706433  
017700000001

### **Cloud Metadata Target**

169.254.169.254

This IP often stores:

* Cloud instance metadata  
* Access tokens  
* IAM credentials

## **Allow Lists**

### **What is an Allow List?**

An Allow List permits only approved URLs or patterns.

Example restriction:

https://website.thm

### **Allow List Bypass**

Attackers may use crafted subdomains:

https://website.thm.attacker-domain.thm

The request still begins with the approved string.

## **Open Redirects**

### **What is an Open Redirect?**

An Open Redirect automatically forwards users to another URL.

Example:

https://website.thm/link?url=https://tryhackme.com

### **SSRF Exploitation**

Attackers abuse open redirects to bypass SSRF filters.

Example flow:

1. Allowed domain passes validation  
2. Server follows redirect  
3. Request reaches attacker-controlled destination

## **SSRF Lab Walkthrough**

### **Vulnerable Feature**

The Acme IT Support application allows users to choose avatars.

The avatar path is controlled through form input values.

### **Initial Attempt**

private

Blocked due to a deny list.

### **Directory Traversal Bypass**

Payload:

x/../private

### **Why It Works**

The server resolves:

x/../private

to:

/private

This bypasses the deny list restriction.

### **Extracting the Flag**

The response:

* Is embedded using a Data URI  
* Encoded in Base64

Attackers:

1. View page source  
2. Copy Base64 data  
3. Decode the content  
4. Retrieve the flag

## **Additional Information**

### **Common SSRF Targets**

| Target | Purpose |
| ----- | ----- |
| 127.0.0.1 | Localhost services |
| 169.254.169.254 | Cloud metadata |
| Internal APIs | Administrative access |
| Redis/MongoDB | Database interaction |

### **SSRF Detection Tips**

Look for:

* URL parameters  
* File fetch functionality  
* Webhooks  
* Avatar/image imports  
* PDF generators

## **Defensive Measures**

Developers should:

* Validate and sanitize URLs  
* Restrict outbound requests  
* Disable unnecessary protocols  
* Use allowlists carefully  
* Block access to internal IP ranges  
* Prevent redirect following

## **Questions and Answers**

What does SSRF stand for?  
 **Server-Side Request Forgery**

As opposed to a regular SSRF, what is the other type?  
 **Blind SSRF**

What is the flag from the SSRF Examples site?  
 **THM{SSRF\_MASTER}**

Based on simple observation, which URL is more likely vulnerable to SSRF?

https://website.thm/fetch-file.php?fname=242533.pdf\&srv=filestorage.cloud.thm\&port=8001

What method can be used to bypass strict rules?  
 **Open Redirect**

What IP address may contain sensitive data in a cloud environment?  
 **169.254.169.254**

What type of list is used to permit only certain input?  
 **Allow List**

What type of list is used to stop certain input?  
 **Deny List**

What is the flag from the /private directory?  
 **THM{SSRF\_BYPASS\_COMPLETE}**

