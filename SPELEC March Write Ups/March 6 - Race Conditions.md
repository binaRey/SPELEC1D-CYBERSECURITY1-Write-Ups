# **Program, Process, and Thread Overview**

## **Overview**

In operating systems and computer science, programs, processes, and threads are fundamental concepts that explain how applications run and perform tasks. A **program** is a static set of instructions, a **process** is a program currently being executed, and a **thread** is a lightweight execution unit inside a process. These concepts help systems manage multitasking, resource allocation, and efficient application performance.

Modern applications such as web servers, browsers, and mobile apps rely heavily on multi-threading and process management to handle multiple users and operations simultaneously.

## **Key Takeaways**

* A **program** is a collection of instructions written to perform a task.  
* A **process** is an active instance of a program in execution.  
* A **thread** is a smaller execution unit inside a process.  
* Multi-threading allows applications to handle several tasks at the same time.  
* Processes have different states such as New, Ready, Running, Waiting, and Terminated.  
* Threads share resources and memory within the same process.  
* Web servers commonly use threads and worker processes to improve performance.

## **Programs**

A program is a static file containing instructions written in a programming language. It does not perform any action until executed.

### **Coffee Recipe Analogy**

* A coffee recipe contains steps for preparing coffee.  
* The recipe alone does nothing unless someone follows the instructions.  
* Similarly, a computer program remains inactive until it runs.

### **Flask Example**

The provided Flask code:

* Creates a simple web server.  
* Listens on port 8080.  
* Returns a webpage displaying:  
  * “Hello, World\!”

The code itself is only a set of instructions until executed.

### **Additional Information**

* Programs are stored on disk as executable files.  
* Examples include:  
  * Google Chrome  
  * Microsoft Word  
  * Python scripts  
  * Mobile applications

## **Processes**

A process is a program currently being executed by the operating system.

Unlike programs, processes are dynamic and actively consume system resources.

### **Main Components of a Process**

* **Program Code**  
  * The executable instructions.  
* **Memory**  
  * Temporary storage for variables and runtime data.  
* **State**  
  * Represents the current activity of the process.

### **Process States**

* **New**  
  * The process has just been created.  
* **Ready**  
  * Waiting for CPU allocation.  
* **Running**  
  * Currently executing instructions.  
* **Waiting**  
  * Waiting for an event or I/O operation.  
* **Terminated**  
  * Execution has completed.

### **Flask Process Example**

When the Flask server runs:

* A process is created.  
* The process waits for incoming HTTP requests.  
* Once a request arrives:  
  * It moves to the Ready state.  
  * Then Running state.  
  * Sends the webpage response.  
  * Returns to Waiting state.

### **Additional Facts**

* Each running application on a computer is usually a process.  
* Processes are isolated from one another for security and stability.  
* Multiple processes can run simultaneously in modern operating systems.

## **Threads**

A thread is the smallest execution unit within a process.

Threads allow a process to perform multiple tasks concurrently.

### **Espresso Machine Analogy**

* The espresso machine represents the process.  
* Each portafilter represents a thread.  
* Multiple portafilters allow several coffee orders to be prepared at once.

### **Benefits of Threads**

* Faster task handling.  
* Better responsiveness.  
* Efficient resource sharing.  
* Improved multitasking.

### **Multi-threading**

Multi-threading allows one process to create multiple threads to handle several tasks simultaneously.

#### **Serial Processing**

* Handles one request at a time.  
* Other requests wait in a queue.

#### **Parallel Processing**

* Multiple threads handle multiple requests simultaneously.  
* Reduces waiting time.

### **Gunicorn Example**

The Gunicorn command:

gunicorn \--workers=4 \--threads=2 \-b 0.0.0.0:8080 app:app

Means:

* 4 worker processes are created.  
* Each worker has 2 threads.  
* Multiple requests can be processed concurrently.

### **Additional Information**

* Threads share the same memory space inside a process.  
* Creating threads is generally faster than creating processes.  
* Excessive threads can still consume CPU and memory resources.

## **Important Concepts and Facts**

### **Port Usage**

* A TCP/UDP port can only be bound to one process at a time.  
* Example:  
  * Port 8080 can only be controlled by one server process.

### **Web Servers**

Modern web servers commonly use:

* Multiple worker processes  
* Multi-threading  
* Load balancing

### **Common Applications Using Multi-threading**

* Web browsers  
* Video games  
* Streaming applications  
* Database servers  
* Chat applications

### **Difference Between Program, Process, and Thread**

| Concept | Description |
| ----- | ----- |
| Program | Static set of instructions |
| Process | Running instance of a program |
| Thread | Lightweight execution unit inside a process |

## **Questions and Answers**

**You downloaded an instruction booklet on how to make an origami crane. What would this instruction booklet resemble in computer terms?**  
 Program

**What is the name of the state where a process is waiting for an I/O event?**  
 Waiting

# **Race Conditions and TOCTOU Vulnerabilities Overview**

## **Overview**

A race condition occurs when multiple threads or processes access and manipulate shared resources simultaneously without proper synchronization. Because the operations happen concurrently, the final result becomes unpredictable and may lead to incorrect behavior, inconsistent data, or security vulnerabilities.

One common type of race condition is the **Time-of-Check to Time-of-Use (TOCTOU)** vulnerability, where a value is checked and later used, but another process modifies the value between those two operations.

Race conditions are particularly dangerous in:

* Banking systems  
* Authentication systems  
* Web applications  
* Multi-threaded applications  
* Database operations

## **Key Takeaways**

* Race conditions occur when multiple operations access shared resources simultaneously.  
* Results become unpredictable due to timing differences between threads.  
* TOCTOU vulnerabilities happen when data changes between validation and execution.  
* Shared variables, databases, and files are common targets.  
* Multi-threaded applications are especially vulnerable without synchronization.  
* Proper locking and transaction controls help prevent race conditions.

## **Real-World Analogy: Restaurant Reservation**

Imagine:

* Two hosts at a restaurant handle reservations simultaneously.  
* Both check if Table 17 is available.  
* Neither places the “Reserved” sign immediately.  
* Both customers end up reserving the same table.

This situation demonstrates a race condition because:

* Two separate operations accessed the same shared resource.  
* Timing differences caused inconsistent results.

### **Important Concept**

The issue occurred because:

* Checking availability  
* Updating reservation status

Were not performed atomically (as one secure operation).

## **Race Condition Example A**

### **Scenario**

* Bank account balance: $100  
* Two threads withdraw money simultaneously.

### **Thread Actions**

* Thread 1 withdraws $45  
* Thread 2 withdraws $35

### **Problem**

Both threads initially read the same balance:

* $100

Because updates happen at different times:

* Thread 1 calculates remaining balance as $55  
* Thread 2 calculates remaining balance as $65

If Thread 2 overwrites Thread 1’s update:

* The account incorrectly shows $65

### **Security Impact**

* Money was withdrawn twice.  
* Balance deduction only reflected one transaction.

## **Race Condition Example B**

### **Scenario**

* Bank account balance: $75  
* Two threads withdraw $50 simultaneously.

### **Problem**

Both threads:

* Check balance before updates occur.  
* Both believe sufficient funds exist.

### **Result**

* Both withdrawals succeed incorrectly.  
* Account may become negative or inconsistent.

### **Why This Is Dangerous**

This bypasses:

* Transaction validation  
* Financial integrity controls

## **TOCTOU Vulnerability**

TOCTOU stands for:

Time-of-Check to Time-of-Use (TOCTOU)\\text{Time-of-Check to Time-of-Use (TOCTOU)}Time-of-Check to Time-of-Use (TOCTOU)

### **Definition**

A vulnerability where:

1. A system checks a condition.  
2. Another process changes the data.  
3. The system uses outdated information.

### **Common Examples**

* File permission checks  
* Banking transactions  
* Authentication systems  
* Temporary file handling

## **Python Race Condition Example**

The provided Python script demonstrates:

* Two threads modifying the same variable x  
* Concurrent execution  
* Unpredictable scheduling

### **Shared Variable**

x \= 0

Both threads repeatedly increment:

* x \+= 1

### **Observed Behavior**

* Different executions produce different outputs.  
* Either thread may finish first.  
* Thread scheduling is unpredictable.

### **Why This Happens**

The operating system:

* Controls CPU scheduling.  
* Decides which thread executes at a given moment.

Because there is no synchronization:

* Both threads “race” to update the variable.

## **Causes of Race Conditions**

### **Parallel Execution**

Web servers often:

* Handle multiple user requests simultaneously.  
* Access shared resources concurrently.

Without synchronization:

* Data corruption or inconsistent behavior may occur.

### **Database Operations**

Simultaneous database updates can cause:

* Lost updates  
* Incorrect balances  
* Duplicate transactions

### **Third-Party Services**

External APIs and libraries may:

* Fail under concurrent access  
* Produce inconsistent results

## **Common Shared Resources**

Examples include:

* Database records  
* Shared memory  
* Files  
* Session variables  
* Global variables  
* In-memory caches

## **Preventing Race Conditions**

### **Locking Mechanisms**

Locks ensure:

* Only one thread accesses a resource at a time.

### **Transaction Isolation**

Databases use:

* Transactions  
* Isolation levels  
* Atomic operations

### **Synchronization Techniques**

Examples:

* Mutexes  
* Semaphores  
* Monitors  
* Thread-safe programming

### **Validation and Rechecking**

Applications should:

* Revalidate data before use.  
* Avoid trusting stale information.

## **Additional Information and Facts**

### **Why Race Conditions Are Dangerous**

They can lead to:

* Financial fraud  
* Authentication bypass  
* Data corruption  
* Privilege escalation  
* System crashes

### **Common Targets in Cybersecurity**

Attackers often exploit race conditions in:

* File uploads  
* Temporary files  
* Password reset mechanisms  
* Banking systems  
* Ticket booking systems

### **Multi-threaded Web Applications**

Modern web applications:

* Frequently use parallel processing.  
* Increase performance but also increase race condition risks.

## **Questions and Answers**

**Does the presented Python script guarantee which thread will reach 100% first? (Yea/Nay)**  
 Nay

**In the second execution of the Python script, what is the name of the thread that reached 100% first?**  
 Thread-1

# **Web Application Architecture and Race Conditions Overview**

## **Overview**

Modern web applications operate using a **client-server architecture** and commonly follow a **multi-tier design**. These systems process multiple requests simultaneously, making them vulnerable to race conditions when shared resources are accessed concurrently without proper synchronization.

Race conditions occur because applications transition through several internal states while processing requests. During these short time windows, attackers may send multiple requests simultaneously to manipulate application logic before the system updates its state securely.

Understanding application architecture and state transitions is essential when analyzing race condition vulnerabilities.

## **Key Takeaways**

* Web applications follow a client-server model.  
* Multi-tier architectures separate frontend, backend, and database operations.  
* Applications move through multiple internal states while processing requests.  
* Race conditions occur during small timing windows between these states.  
* Business logic operations like money transfers and coupon validation are common targets.  
* Simultaneous requests may bypass security controls before state updates complete.  
* Tools such as Burp Suite help exploit race conditions by sending requests milliseconds apart.

## **Client-Server Model**

Web applications rely on communication between:

* Clients  
* Servers

### **Client**

The client:

* Initiates requests.  
* Displays content to users.

Examples:

* Web browsers  
* Mobile apps  
* Desktop applications

### **Server**

The server:

* Processes incoming requests.  
* Executes business logic.  
* Retrieves or updates data.

Examples:

* Web servers  
* Application servers  
* API servers

### **Example Workflow**

1. User visits a website.  
2. Browser sends an HTTP request.  
3. Server processes the request.  
4. Database is queried if needed.  
5. Server returns the response.  
6. Browser renders the content.

## **Typical Web Application Architecture**

Most web applications use a **three-tier architecture**.

### **Presentation Tier**

Responsible for:

* User interface  
* Rendering webpages

Technologies:

* HTML  
* CSS  
* JavaScript

### **Application Tier**

Handles:

* Business logic  
* Authentication  
* Request processing

Technologies:

* PHP  
* Node.js  
* Python  
* Java

### **Data Tier**

Responsible for:

* Data storage  
* Database operations

Common DBMS:

* MySQL  
* PostgreSQL

## **Understanding Application States**

Applications transition through several states while processing actions.

These states create:

* Timing gaps  
* Synchronization challenges  
* Race condition opportunities

## **Money Transfer Example**

### **Workflow**

1. User clicks “Confirm Transfer”  
2. Application checks account balance  
3. Database validates available funds  
4. Transfer is processed  
5. Account balance updates

## **Original State Diagram**

Initially, the process appears to have only:

* Amount not sent  
* Amount sent

Total States:

222

## **Updated State Diagram**

A more realistic model includes:

* Amount not sent  
* Checking balance and limits  
* Amount sent

Total States:

333

### **Why This Matters**

The checking phase creates a vulnerable timing window where:

* Multiple requests may pass validation simultaneously.

## **Coupon Validation Example**

### **Workflow**

1. User enters coupon code  
2. Application validates coupon  
3. System checks constraints  
4. Discount gets applied  
5. Cart total recalculates

## **Initial States**

Initially:

* Coupon not applied  
* Coupon applied

## **Expanded State Model**

The final realistic diagram contains:

* Coupon not applied  
* Checking coupon applicability  
* Checking coupon validity  
* Checking coupon constraints  
* Recalculating total  
* Coupon applied

Total States:

555

## **Why Race Conditions Happen**

Race conditions occur because:

* State transitions take time.  
* Security checks are not instantaneous.  
* Multiple requests may reach the server before updates complete.

### **Example**

An attacker may:

* Submit the same coupon multiple times rapidly.  
* Exploit the short delay before the coupon is marked as “used”.

## **Time Window Vulnerability**

The vulnerable period exists between:

* Validation  
* Final state update

This tiny delay is often measured in:

* Milliseconds

Attackers exploit this timing gap using:

* Parallel requests  
* Automated tools  
* Multi-threaded attacks

## **Common Targets for Race Conditions**

### **Financial Systems**

* Money transfers  
* Wallet balances  
* Payment processing

### **E-commerce Systems**

* Coupon reuse  
* Limited inventory purchases  
* Discount stacking

### **Authentication Systems**

* Password reset tokens  
* Session handling  
* OTP verification

### **Ticketing Systems**

* Double bookings  
* Seat reservations

## **Exploitation Tools**

A common tool used for race condition testing is:

* Burp Suite

### **Why Burp Suite?**

It allows:

* Parallel HTTP requests  
* Request repetition  
* Timing manipulation  
* Intruder and Turbo Intruder attacks

## **Additional Information and Facts**

### **Why Timing Matters**

Even vulnerable applications may:

* Only expose the flaw for milliseconds.

### **Modern Web Applications**

High-performance applications:

* Use multi-threading  
* Process concurrent requests  
* Increase race condition risks

### **Database Transactions**

Proper database transaction handling helps prevent:

* Duplicate processing  
* Inconsistent updates  
* Lost data

### **Synchronization Methods**

Common protections include:

* Mutex locks  
* Database row locking  
* Atomic operations  
* Transaction isolation levels

## **Questions and Answers**

**How many states did the original state diagram of “validating and conducting money transfer” have?**  
 2

**How many states did the updated state diagram of “validating and conducting money transfer” have?**  
 3

**How many states did the final state diagram of “validating coupon codes and applying discounts” have?**  
 5

# **Race Condition Vulnerability Exploitation**

## **Overview**

A race condition vulnerability occurs when multiple requests or processes interact with the same resource simultaneously, causing unintended behavior due to improper synchronization. These vulnerabilities commonly affect applications involving:

* Banking transactions  
* Credit transfers  
* Coupon systems  
* Inventory management  
* Authentication systems

Race conditions are especially dangerous because attackers can exploit tiny timing windows to:

* Duplicate transactions  
* Bypass payment limits  
* Reuse one-time actions  
* Gain unauthorized credits or funds  
* Cause inconsistent database states

In this lab, the goal was to exploit the vulnerable banking application by sending multiple transfer requests simultaneously using Burp Suite Repeater’s **Send group in parallel** feature.

## **Key Concepts**

### **Important Terms**

* **Program** – Static instructions stored on disk  
* **Process** – A program currently running  
* **Thread** – Lightweight execution unit inside a process  
* **Race Condition** – Multiple threads/processes competing to access shared resources simultaneously  
* **TOCTOU (Time-of-Check to Time-of-Use)** – A flaw where validation occurs before execution, allowing changes during the delay window

### **Common Causes of Race Conditions**

* Parallel request handling  
* Shared database records  
* Missing synchronization/locking  
* Multi-threaded web servers  
* Delayed state updates

### **Burp Suite Repeater Techniques**

#### **Sequential Requests**

* Requests sent one after another  
* Usually harder to exploit race conditions  
* Server processes requests individually

#### **Parallel Requests**

* Requests sent simultaneously  
* Best method for exploiting timing windows  
* Burp uses synchronization techniques such as:  
  * Last-byte synchronization (HTTP/1)  
  * Single-packet attacks (HTTP/2)

---

# **Lab Walkthrough – Credit Transfer Exploit**

## **Target Application**

The vulnerable mobile operator application allows users to transfer phone credits between accounts.

### **Test Accounts**

#### **User 1**

* Username: 07799991337  
* Password: pass1234

#### **User 2**

* Username: 07113371111  
* Password: pass1234

---

# **Exploitation Steps**

## **Step 1 – Login**

* Open Burp Suite  
* Launch Burp Browser  
* Navigate to:

   http://MACHINE\_IP:8080

* Login using either account

---

## **Step 2 – Observe Normal Requests**

Navigate to:

Pay & Recharge → Transfer

Transfer a small amount such as:

$1.5

Observe the POST request in:

Proxy → HTTP History

Typical request:

POST /transfer HTTP/1.1

Response confirms successful transfer.

---

## **Step 3 – Send Request to Repeater**

* Right-click the POST request  
* Select:

Send to Repeater  
---

## **Step 4 – Create Repeater Group**

Inside Repeater:

1. Create a new tab group  
2. Add the transfer request  
3. Duplicate the request multiple times  
   * Around 20 duplicates recommended

---

## **Step 5 – Send Requests in Parallel**

Click the dropdown beside **Send** and choose:

Send group in parallel

This causes all transfer requests to hit the server nearly simultaneously.

Because the application checks the balance before updating it, multiple requests succeed before the balance changes.

---

## **Vulnerability Result**

The application incorrectly processed all simultaneous requests.

Effects observed:

* Multiple transfers succeeded  
* Account balance exceeded intended limits  
* Race condition successfully exploited

---

# **Important Technical Observation**

### **Why Sequential Sending Failed**

Sequential requests allowed the application enough time to:

1. Validate balance  
2. Update database  
3. Reject later requests

### **Why Parallel Sending Worked**

Parallel requests reached the server during the vulnerable timing window before balance updates completed.

This exploited the TOCTOU flaw.

---

# **Final Answer**

## **What is the flag that you obtained?**

THM{R4c3\_C0nd1t10n\_Succ3ss}  
---

# **Challenge Walkthrough – Banking Application**

## **Target**

http://MACHINE\_IP:5000

### **Accounts**

#### **Account 1**

* Username: 4621  
* Password: blueApple

#### **Account 2**

* Username: 6282  
* Password: whiteHorse

#### **Account 3**

* Username: 9317  
* Password: greenOrange

---

# **Exploitation Strategy**

The process is identical:

1. Login  
2. Perform a normal transfer  
3. Capture POST request  
4. Send request to Repeater  
5. Duplicate request multiple times  
6. Use:

    Send group in parallel

7. Abuse simultaneous transactions  
8. Increase account balance beyond $1000

---

# **Final Challenge Answer**

## **What flag did you obtain after getting an account’s balance above $1000?**

THM{B4nk\_R4c3\_Expl01t}  
---

# **Key Takeaways**

* Race conditions occur because systems fail to properly synchronize shared resources.  
* Even milliseconds matter during exploitation.  
* Burp Suite Repeater is highly effective for testing timing-based vulnerabilities.  
* Parallel requests are far more dangerous than sequential requests.  
* Proper server-side locking and transaction isolation are critical defenses.

---

# **Common Defenses Against Race Conditions**

## **Synchronization Mechanisms**

* Mutexes  
* Locks  
* Semaphores

## **Database Protections**

* Transaction isolation  
* Atomic operations  
* Row-level locking

## **Application-Level Mitigations**

* Request queuing  
* Idempotency tokens  
* Double-spend prevention logic  
* Proper validation after execution

---

# **Important Security Insight**

Race conditions are often difficult to detect because applications behave normally under standard usage. Exploitation typically requires:

* Precise timing  
* Concurrent requests  
* Understanding application state transitions  
* Specialized tooling such as Burp Suite

This makes race condition vulnerabilities both subtle and extremely impactful in real-world systems.

