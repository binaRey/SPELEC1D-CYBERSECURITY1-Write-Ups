# **Introduction to Intruder**

Intruder is Burp Suite’s built-in fuzzing and automation tool used for repetitive request testing and payload injection. It is commonly used for:

* Brute-forcing login forms  
* Fuzzing endpoints and parameters  
* Testing input validation  
* Enumerating directories and virtual hosts  
* Automating payload injection attacks

Intruder works by taking a captured request (usually from the Proxy module) and inserting payloads into predefined positions.

The Intruder interface contains four main sub-tabs:

* **Positions** – Defines payload insertion points and attack types  
* **Payloads** – Configures payload lists and processing rules  
* **Resource Pool** – Manages resources (mainly useful in Professional edition)  
* **Settings** – Controls attack behavior and response handling

A common use of Intruder is fuzzing, which involves sending many modified requests to identify valid responses, hidden endpoints, or vulnerabilities.

Question: In which Intruder tab can we define the "Attack type" for our planned attack?  
 Answer: Positions

# **Positions Tab**

The **Positions** tab is where Intruder determines where payloads will be inserted into the request.

Payload positions are marked using the symbol:

§

Burp Suite automatically attempts to identify potential insertion points and surrounds them with section markers (§).

Important controls in the Positions tab:

* **Add §** – Manually create payload positions  
* **Clear §** – Remove all payload positions  
* **Auto §** – Automatically detect likely positions again

These controls help customize how Intruder modifies requests during an attack.

Question: What symbol defines the start and the end of a payload position?  
 Answer: §

# **Payloads Tab**

The **Payloads** tab allows configuration of the data that Intruder injects into payload positions.

It consists of four sections:

### **Payload Sets**

* Selects the payload set  
* Defines payload type  
* Multiple sets are used in Pitchfork and Cluster Bomb attacks

### **Payload Settings**

* Adds or removes payload entries  
* Loads payloads from files  
* Supports different payload generation methods

### **Payload Processing**

* Applies transformations to payloads before sending  
* Examples:  
  * Add prefix  
  * Add suffix  
  * Capitalize payloads  
  * Match-and-replace operations

### **Payload Encoding**

* Controls URL encoding behavior  
* Allows custom encoding settings

Question: Which Payload processing rule could we use to add characters at the end of each payload in the set?  
 Answer: Add suffix

# **Attack Types**

Intruder provides four attack types, each designed for different testing scenarios.

## **Sniper**

* Default attack type  
* Uses a single payload set  
* Inserts one payload into one position at a time  
* Best for focused testing of individual parameters

## **Battering Ram**

* Uses one payload set  
* Inserts the same payload into all positions simultaneously  
* Useful for race conditions or mirrored parameters

## **Pitchfork**

* Uses multiple payload sets  
* Sends corresponding payloads together  
* Useful for credential stuffing

## **Cluster Bomb**

* Uses multiple payload sets  
* Tests every possible combination of payloads  
* Useful for brute-force attacks when username-password mappings are unknown

Question: What attack type cycles through the payloads inserting one payload at a time into each position defined in the request?  
 Answer: Sniper

# **Battering Ram Attack**

The **Battering Ram** attack type inserts the same payload into every position simultaneously.

Example request template:

username=§pentester§\&password=§Expl01ted§

Example payload list:

admin  
Guest

Generated requests:

username=admin\&password=admin  
username=Guest\&password=Guest

This attack type is useful when the same value must be tested across multiple parameters.

Question: What would the body parameters of the first request that Burp Suite sends be?  
 Answer: username=admin\&password=admin

# **Pitchfork Attack**

The **Pitchfork** attack type uses one payload set per position and iterates through them simultaneously.

Example:

Payload Set 1:

joel  
harriet  
alex

Payload Set 2:

J03l  
Emma1815  
Sk1ll

Generated requests:

username=joel\&password=J03l  
username=harriet\&password=Emma1815  
username=alex\&password=Sk1ll

Pitchfork is ideal for:

* Credential stuffing  
* Testing paired values  
* Simultaneous multi-position attacks

Question: What is the maximum number of payload sets we can load into Intruder in Pitchfork mode?  
 Answer: 20

# **Cluster Bomb Attack**

The **Cluster Bomb** attack type tests every possible combination of payloads from multiple payload sets.

Example payload sets:

### **Usernames**

joel  
harriet  
alex

### **Passwords**

J03l  
Emma1815  
Sk1ll

Generated requests include every possible pairing:

username=joel\&password=J03l  
username=harriet\&password=J03l  
username=alex\&password=J03l  
username=joel\&password=Emma1815  
...

Cluster Bomb attacks can generate extremely large numbers of requests because every combination is tested.

Formula:

Total Requests \= Payload Set 1 × Payload Set 2 × Payload Set 3 ...

Example calculation:

100 × 2 × 30 \= 6000

Question: How many requests will Intruder make using these payload sets in a Cluster bomb attack?  
 Answer: 6000

# **Credential Stuffing Attack**

In this exercise, the goal is to gain access to the support portal located at:

http://MACHINE\_IP/support/login

The login form contains standard username and password fields without additional security protections such as CAPTCHA, MFA, or rate limiting.

Example source code:

\<form method="POST"\>  
   \<div class="form-floating mb-3"\>  
       \<input class="form-control" type="text" name=username placeholder="Username" required\>  
       \<label for="username"\>Username\</label\>  
   \</div\>

   \<div class="form-floating mb-3"\>  
       \<input class="form-control" type="password" name=password placeholder="Password" required\>  
       \<label for="password"\>Password\</label\>  
   \</div\>

   \<div class="d-grid"\>  
       \<button class="btn btn-primary btn-lg" type="submit"\>  
           Login\!  
       \</button\>  
   \</div\>  
\</form\>

Instead of performing a normal brute-force attack, a credential stuffing attack is used because leaked usernames and passwords are already available.

The leaked credential archive can be downloaded with:

wget http://MACHINE\_IP:9999/Credentials/BastionHostingCreds.zip

Extracted files include:

* emails.txt  
* usernames.txt  
* passwords.txt  
* combined.txt

The attack setup process:

1. Capture a login request using Burp Proxy  
2. Send the request to Intruder  
3. Select only:  
   * username parameter  
   * password parameter  
4. Set attack type to **Pitchfork**  
5. Load:  
   * usernames.txt into Payload Set 1  
   * passwords.txt into Payload Set 2  
6. Start the attack  
7. Sort results by response length  
8. Identify the shortest response indicating successful authentication

Question: What username and password combination indicates a successful login attempt?  
 Answer: administrator:password123

# **IDOR Ticket Enumeration**

After successfully logging into the support system, tickets can be accessed through endpoints formatted as:

http://MACHINE\_IP/support/ticket/NUMBER

This suggests the possibility of an **IDOR (Insecure Direct Object Reference)** vulnerability because ticket IDs are sequential integers.

Potential scenarios:

* Proper access control prevents unauthorized access  
* Missing authorization checks allow viewing other users’ tickets

To test this, Intruder can be used to fuzz ticket numbers between 1 and 100\.

Recommended attack configuration:

* Attack Type: Sniper  
* Payload Position: Ticket number  
* Payload Type: Numbers  
* Range: 1–100

Successful tickets return:

HTTP 200 OK

By reviewing the responses, one ticket contains the hidden flag.

Question: Which attack type is best suited for this task?  
 Answer: Sniper

Question: What is the flag?  
 Answer: THM{MzUzY2U5NjUwMzUzMmIxNmYwNjRiY2I3}

# **Burp Macros and CSRF-Protected Login**

A more advanced credential stuffing challenge introduces additional protections:

* Dynamic session cookies  
* CSRF tokens

Login endpoint:

http://MACHINE\_IP/admin/login/

Captured response example:

Set-Cookie: session=...  
\<input type="hidden" name="loginToken" value="84c6358bbf1bd8000b6b63ab1bd77c5e"\>

Both values change with every request, making standard Intruder attacks ineffective.

To bypass this:

## **Macro Configuration**

### **Step 1 – Create a Macro**

* Navigate to:  
  * Settings → Sessions → Macros  
* Add a GET request to:  
  * /admin/login/

### **Step 2 – Create Session Handling Rule**

* Go to:  
  * Settings → Sessions → Session Handling Rules  
* Add a new rule  
* Restrict scope to:  
  * Intruder only

### **Step 3 – Configure Macro Actions**

Set the macro to update only:

Parameters:

loginToken

Cookies:

session

## **Intruder Configuration**

Attack Type:

Pitchfork

Payload Sets:

* usernames.txt  
* passwords.txt

The macro automatically refreshes:

* CSRF token  
* Session cookie

This ensures every login attempt uses valid authentication data.

After starting the attack:

* All responses return HTTP 302  
* Successful logins are identified by shorter response lengths

Question: What username and password combination indicates a successful login attempt?  
 Answer: admin:lovely13

