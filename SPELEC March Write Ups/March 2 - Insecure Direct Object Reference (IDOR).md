# **Insecure Direct Object Reference (IDOR)**

## **Overview**

An Insecure Direct Object Reference (IDOR) vulnerability occurs when an application exposes internal object references such as user IDs, filenames, or account numbers without properly verifying whether the requesting user is authorized to access them.

This vulnerability allows attackers to manipulate identifiers in URLs, API requests, cookies, or hidden parameters to access unauthorized data belonging to other users.

In this exercise, the application exposed user information through predictable ID values inside API requests, allowing unauthorized access to other users’ account details.

## **Key Takeaways**

* IDOR vulnerabilities occur because of missing authorization checks.  
* Simply hiding or encoding IDs does not secure an application.  
* APIs and AJAX requests are common places where IDORs are found.  
* Developers must always validate ownership and permissions server-side.  
* Predictable numeric IDs are especially vulnerable to enumeration attacks.

## **Basic IDOR Example**

A user accesses their profile page:

http://online-service.thm/profile?user\_id=1305

By changing the parameter:

http://online-service.thm/profile?user\_id=1000

the attacker gains access to another user's information because the application does not verify whether the requested profile belongs to the logged-in user.

## **Encoded IDs**

Web applications sometimes encode identifiers before sending them to users.

### **Common Encoding Method**

* Base64

### **Why Encoding is Used**

Encoding helps:

* Safely transmit data  
* Convert binary data into readable text  
* Preserve special characters during transmission

### **Important Fact**

Encoding is **not security** because it is reversible.

Example:

VEhNe0JBU0U2NF9FTkNPRElOR30=

Base64 Decoded:

THM{BASE64\_ENCODING}

Attackers can:

1. Decode the value  
2. Modify the data  
3. Re-encode it  
4. Resubmit the request

## **Hashed IDs**

Some applications hash identifiers instead of exposing raw values.

Example:

123 → 202cb962ac59075b964b07152d234b70

### **Common Hashing Algorithms**

* MD5  
* SHA1  
* SHA-256

### **Important Facts**

* Hashes are one-way transformations.  
* Weak hashes can often be cracked using online databases.  
* Predictable hash patterns may still allow IDOR exploitation.

## **Unpredictable IDs**

When IDs are not obvious, testers often:

* Create multiple accounts  
* Compare requests between accounts  
* Swap identifiers between sessions

This helps determine whether authorization checks are properly enforced.

## **Finding Hidden IDOR Endpoints**

IDOR vulnerabilities are not always visible in the browser address bar.

They may exist in:

* AJAX requests  
* API endpoints  
* JavaScript files  
* Hidden parameters

### **Example API Request**

/api/v1/customer?id=3

The application returns JSON data containing:

* User ID  
* Username  
* Email Address

If changing the id parameter displays another user’s information, the endpoint is vulnerable to IDOR.

## **Additional Information**

### **Common IDOR Targets**

* User profiles  
* Support tickets  
* Documents  
* API endpoints  
* Download links  
* Order histories

### **Defensive Measures**

Developers should:

* Perform server-side authorization checks  
* Use indirect object references  
* Implement role-based access control  
* Avoid exposing sequential IDs  
* Validate ownership for every request

## **Questions and Answers**

What is the Flag from the IDOR example website?  
 **THM{IDOR\_VULNERABILITY\_DETECTED}**

What is a common type of encoding used by websites?  
 **base64**

What is a common algorithm used for hashing IDs?  
 **md5**

What is the minimum number of accounts you need to create to check for IDORs between accounts?  
 **2**

What is the username for user id 1?  
 **adam84**

What is the email address for user id 3?  
 **admin@hacker.com**

