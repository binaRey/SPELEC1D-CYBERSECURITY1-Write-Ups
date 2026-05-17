## **Overview: Cybersecurity Vulnerabilities**

A vulnerability is a weakness or flaw in a system, application, or security process that can be exploited by an attacker to gain unauthorized access or perform unintended actions. These weaknesses can arise from poor design, incorrect implementation, misconfigurations, or human error.

Organizations like NIST define vulnerabilities as flaws in systems or controls that can be triggered or exploited by threats.

Understanding vulnerability types is essential for identifying risks and improving system security.

---

## **Key Takeaways**

* A vulnerability is any weakness that can be exploited by an attacker.  
* Vulnerabilities exist in systems, applications, configurations, and human behavior.  
* Exploitation can lead to:  
  * Unauthorized access  
  * Data leakage  
  * Privilege escalation  
* Vulnerabilities often come from:  
  * Poor design decisions  
  * Misconfigurations  
  * Weak authentication practices  
  * Human error or manipulation

---

## **Main Categories of Vulnerabilities**

* **Operating System Vulnerabilities**  
  * Found in OS-level components  
  * Can lead to privilege escalation or system compromise  
* **(Mis)Configuration-Based Vulnerabilities**  
  * Caused by incorrect system or service configurations  
  * Example: exposed admin panels or sensitive data  
* **Weak or Default Credentials**  
  * Systems using factory-set or easily guessable passwords  
  * Common in poorly secured applications  
* **Application Logic Vulnerabilities**  
  * Arise from flawed application design  
  * Example: broken authentication or authorization logic  
* **Human-Factor Vulnerabilities**  
  * Exploit human behavior rather than technical flaws  
  * Example: phishing attacks or social engineering

---

## **Security Insight**

* Many real-world breaches occur due to **simple misconfigurations or weak credentials**  
* Even secure systems can be compromised if user behavior is exploited  
* Effective security requires addressing both technical and human factors

---

## **Questions**

An attacker has been able to upgrade the permissions of their system account from "user" to "administrator". What type of vulnerability is this?

Operating System

You manage to bypass a login panel using cookies to authenticate. What type of vulnerability is this?

Application Logic

## **Overview: Vulnerability Management and Scoring Frameworks**

Vulnerability management is the process of identifying, evaluating, prioritizing, and remediating security weaknesses in systems and applications. Since it is impractical to fix every vulnerability, organizations focus on addressing those that pose the highest risk.

To support decision-making, vulnerability scoring frameworks are used to measure severity and risk, helping security teams prioritize remediation efforts effectively.

---

## **Key Takeaways**

* Vulnerability management focuses on **prioritizing risks, not fixing everything**  
* Only a small percentage of vulnerabilities are actively exploited in real-world attacks  
* Scoring frameworks help determine which vulnerabilities are most dangerous  
* Two major frameworks:  
  * CVSS (Common Vulnerability Scoring System)  
  * VPR (Vulnerability Priority Rating)

---

## **Vulnerability Scoring Frameworks**

### **CVSS (Common Vulnerability Scoring System)**

* First introduced in **2005**  
* Widely used, open, and standardized framework  
* Measures severity based on:  
  * Ease of exploitation  
  * Availability of exploits  
  * Impact on CIA triad (Confidentiality, Integrity, Availability)

**Severity Ratings:**

* Low: 0.1 – 3.9  
* Medium: 4.0 – 6.9  
* High: 7.0 – 8.9  
* Critical: 9.0 – 10.0

**Strengths:**

* Free and widely adopted  
* Standardized across organizations  
* Recommended by security bodies like NIST

**Limitations:**

* Focuses heavily on theoretical severity rather than real-world risk  
* Does not dynamically update often  
* Not designed primarily for prioritization

---

### **VPR (Vulnerability Priority Rating)**

* Developed by **Tenable**  
* Focuses on **real-world risk and prioritization**  
* Evaluates whether a vulnerability is relevant to the organization

**Key Features:**

* Risk-driven approach  
* Dynamic scoring (changes over time)  
* Considers over 150 factors  
* Excludes irrelevant vulnerabilities for a given environment

**Severity Ratings:**

* Low: 0.0 – 3.9  
* Medium: 4.0 – 6.9  
* High: 7.0 – 8.9  
* Critical: 9.0 – 10.0

**Strengths:**

* Highly practical for real-world security teams  
* Prioritizes based on actual exploit risk  
* Continuously updated scoring

**Limitations:**

* Not open-source  
* Requires commercial tools/platforms  
* Less emphasis on CIA triad compared to CVSS

---

## **Security Insight**

* CVSS helps define **how severe a vulnerability is**  
* VPR helps determine **how urgently it should be fixed**  
* Modern organizations often use a combination of both approaches  
* Prioritization is essential because fixing all vulnerabilities is unrealistic

---

## **Questions**

What year was the first iteration of CVSS published?

2005

If you wanted to assess vulnerability based on the risk it poses to an organisation, what framework would you use?

VPR

If you wanted to use a framework that was free and open-source, what framework would that be?

CVSS

## **Overview: Vulnerability Databases (NVD & Exploit-DB)**

Cybersecurity professionals rely on public vulnerability databases to identify, analyze, and understand known security flaws in software and systems. Two of the most widely used resources are the **National Vulnerability Database (NVD)** and **Exploit-DB**.

These platforms help security researchers track vulnerabilities, review exploit techniques, and study proof-of-concept (PoC) implementations that demonstrate real-world attacks.

---

## **Key Takeaways**

* Vulnerabilities are publicly tracked using standardized identifiers called **CVE (Common Vulnerabilities and Exposures)**.  
* CVE format follows: CVE-YEAR-IDNUMBER  
* Security databases help professionals:  
  * Identify known vulnerabilities  
  * Understand exploit techniques  
  * Study real-world attack examples  
* Two major resources:  
  * NVD → structured vulnerability database  
  * Exploit-DB → exploit and PoC repository

---

## **Core Terms**

* **Vulnerability**  
  * A flaw or weakness in a system or application that can be exploited  
* **Exploit**  
  * A method, tool, or action that takes advantage of a vulnerability  
* **Proof of Concept (PoC)**  
  * A demonstration showing how a vulnerability can be exploited

---

## **NVD (National Vulnerability Database)**

* Maintained by the U.S. government  
* Central repository of publicly disclosed CVEs  
* Allows filtering by:  
  * Software type  
  * Date of disclosure  
  * Severity and category

**Key Characteristics:**

* Highly structured and official database  
* Useful for tracking vulnerability trends  
* Contains CVSS scores and technical details

**Limitation:**

* Not ideal for finding practical exploit code or attack methods

---

## **Exploit-DB**

* Public repository of real-world exploits and PoCs  
* Contains:  
  * Exploit scripts  
  * Vulnerability demonstrations  
  * Code examples for testing

**Key Characteristics:**

* Organized by software name, version, and author  
* Very useful for penetration testing and research  
* Focuses on practical exploitation rather than classification

**Strength:**

* Provides working examples of how vulnerabilities are exploited

---

## **Security Insight**

* NVD helps you understand **what is vulnerable**  
* Exploit-DB helps you understand **how it is exploited**  
* Together, they provide a complete vulnerability research workflow  
* Security analysts often cross-reference both for validation and testing

---

## **Questions**

Using NVD, how many CVEs were published in July 2021?

1989

Who is the author of Exploit-DB?

OffSec

## **Overview: Using Version Disclosure for Exploit Research**

In cybersecurity assessments, small pieces of information can lead to discovering much larger vulnerabilities. One common starting point is **version disclosure**, where an application unintentionally reveals its software name and version number.

This information becomes valuable because it allows security researchers or attackers to search vulnerability databases and identify known exploits that match that specific version.

---

## **Key Takeaways**

* Version disclosure reveals:  
  * Application name  
  * Software version number  
* This information is often unintentionally exposed through:  
  * Web pages  
  * Error messages  
  * HTTP headers  
  * Application banners  
* Once identified, it can be used to:  
  * Search Exploit-DB for matching exploits  
  * Identify known CVEs affecting that version  
* Small information leaks can lead to full system compromise when combined with other vulnerabilities

---

## **Vulnerability Workflow Insight**

* Step 1: Discover exposed application details (name \+ version)  
* Step 2: Use Exploit-DB or NVD to research known vulnerabilities  
* Step 3: Identify matching exploits for that exact version  
* Step 4: Test or analyze exploit feasibility

---

## **Security Insight**

* Even minor information exposure can significantly increase attack surface  
* Version numbers are highly sensitive in security contexts  
* Proper configuration should avoid exposing software versions publicly  
* Attackers rely heavily on version-based targeting during reconnaissance

---

## **Questions**

What type of vulnerability did we use to find the name and version of the application in this example?

Version Disclosure

## **Overview: Guided Exploitation (ACKme IT Service – Training Scenario)**

This task is a simulated penetration testing exercise where a junior tester follows a structured workflow to identify and exploit a vulnerability in a deliberately vulnerable web application. The goal is to understand how reconnaissance, information gathering, and exploitation techniques are combined in a real engagement.

The scenario demonstrates how seemingly small issues (like version disclosure or exposed services) can be leveraged to discover and exploit known vulnerabilities, ultimately leading to flag retrieval.

---

## **Key Takeaways**

* Real penetration tests follow a **step-by-step methodology**, not random attacks.  
* Information gathering is critical before exploitation.  
* Even basic vulnerabilities can lead to full compromise when chained together.  
* Tools and databases like:  
  * Nmap (reconnaissance)  
  * Exploit-DB (exploit research)  
  * NVD (vulnerability confirmation)  
     are commonly used together in engagements.

---

## **Engagement Flow (Conceptual)**

* Deploy and access the target application  
* Perform reconnaissance to identify:  
  * Open ports  
  * Web services  
  * Application version details  
* Identify potential vulnerability using disclosed information  
* Search Exploit-DB for matching exploits  
* Apply or simulate exploitation steps  
* Retrieve the flag from the compromised system or web interface

---

## **Security Insight**

* Vulnerability exploitation is often **not a single step**, but a chain of discoveries  
* Information disclosure (like version numbers) is a common entry point  
* Attackers rely heavily on public exploit databases for known vulnerabilities  
* Proper security hardening would include:  
  * Removing version banners  
  * Patching known vulnerabilities  
  * 

**Question**  
Follow along with the showcase of exploiting ACKme’s application to the end to retrieve a flag. What is this flag?

THM{ACKME\_ENGAGEMENT}