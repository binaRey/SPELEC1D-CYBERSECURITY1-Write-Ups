## **Overview: TryHackMe Net Sec Challenge Walkthrough (Network Security Practice Lab)**

This challenge focuses on practical network security techniques used in reconnaissance, service enumeration, and credential attacks. It reinforces how attackers and security testers identify open services, extract information from banners, and attempt password attacks using automated tools.

The lab simulates a real-world environment where multiple services (HTTP, FTP, SSH, Telnet) are exposed, requiring systematic scanning and analysis to discover hidden flags and credentials.

---

## **Key Takeaways**

* Network reconnaissance is the first step in security assessment.  
* Open ports may include both common and non-standard services.  
* Service version detection helps identify vulnerabilities and hidden information.  
* Banner grabbing can expose sensitive data or flags.  
* Automated tools like Hydra can perform fast credential attacks.  
* Stealth scanning techniques help evade detection systems.

---

## **Core Tools Used**

* **Nmap**  
  * Used for port scanning and service discovery  
  * Supports:  
    * Version detection (\-sV)  
    * Script scanning (\-sC)  
    * Full port scan (\-p-)  
    * Stealth scans (e.g., null scan \-sN)  
* **FTP (File Transfer Protocol)**  
  * Used to access and transfer files from remote systems  
  * Often targeted for weak credentials or misconfigurations  
* **Telnet**  
  * Used for connecting to services and grabbing banners  
  * Commonly exposes service information in plaintext  
* **THC Hydra**  
  * Automated brute-force tool  
  * Supports multiple services including FTP and SSH  
  * Uses wordlists like rockyou.txt for password guessing

---

## **Challenge Flow Summary**

* Initial reconnaissance identifies open ports using Nmap.  
* Extended scans reveal hidden services on non-standard ports (e.g., above 10,000).  
* Service enumeration provides:  
  * Version details  
  * Banner information  
  * Hidden flags in HTTP/SSH responses  
* FTP service is analyzed for version and file access.  
* Credential attacks are performed using Hydra with known usernames.  
* Stealth scanning techniques are used to bypass detection systems on a web-based challenge.

---

## **Additional Information & Facts**

* Default Nmap scan only checks the first 1,000 TCP ports, so full scans are necessary for complete visibility.  
* HTTP and SSH banners can sometimes contain embedded flags in CTF environments.  
* Non-standard ports are often used to hide services from basic scans.  
* Hydra increases efficiency by running multiple login attempts in parallel threads.  
* Stealth scans like null scans send unusual packet types to avoid IDS detection.

---

## **Security Insight**

* Attackers rely heavily on **enumeration before exploitation**.  
* Weak or exposed services significantly increase attack surface.  
* Proper network hardening involves:  
  * Closing unused ports  
  * Restricting service exposure  
  * Monitoring scan activity  
* Detection systems can be bypassed with careful scan techniques, showing the importance of layered security.

---

## **Questions**

What is the highest port number being open less than 10,000?

8080

There is an open port outside the common 1,000 ports, it is above 10,000. What is it?

10021

How many TCP ports are open?

6

What is the flag hidden in the HTTP server header?

THM{web\_server\_25352}

What is the flag hidden in the SSH server header?

THM{946219583339}

What is the version of the FTP server?

vsftpd 3.0.5

What is the flag hidden in one of the FTP user accounts?

THM{321452667098}

What is the flag from the stealth scan challenge?

THM{f7443f99}

