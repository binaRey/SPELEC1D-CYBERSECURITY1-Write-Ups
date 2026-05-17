## **Checkpoint Brief Overview**

AI supply chain attacks target the **trust chain** behind machine learning systems. Instead of attacking users directly, attackers hide malicious behaviour inside **model files**, **dependencies**, **policy templates**, or **repository artefacts**. In this TryHackMe scenario, four AI model candidates underwent a **sandboxed evaluation cycle**, where automated scanners detected suspicious activity before deployment.

The investigation focused on **Candidate A**, which showed multiple indicators of compromise during both the **model loading phase** and **inference behaviour**. One of the strongest warning signs was an attempted access to the sensitive Linux file **/etc/passwd**, commonly targeted for credential and system information theft. This indicated possible **credential harvesting** or **system reconnaissance** behaviour embedded inside the model workflow.

Another major issue was the disabled **security\_review\_flag**, meaning an important guardrail protecting security-critical code reviews was intentionally bypassed. Disabling security enforcement mechanisms is a common attacker technique because it weakens detection and approval processes inside CI/CD pipelines.

The model was also governed by a suspicious external policy template called **CommunityReview**. This highlights the growing danger of **prompt template supply chain attacks**, where attackers manipulate AI behaviour through externally sourced templates instead of modifying the model itself. Even when the underlying model appears legitimate, a malicious prompt or policy can silently change decisions, approvals, or security checks.

The attack demonstrated several important AI security risks:

* **Malicious Serialisation** – Hidden code execution inside model files such as pickle payloads.  
* **Repository Manipulation** – Fake or untrusted organisations distributing compromised models.  
* **Policy Template Poisoning** – External review templates altering AI decision-making behaviour.  
* **Guardrail Bypass** – Security protections intentionally disabled during inference or review.  
* **Credential Exfiltration Attempts** – Access to files like /etc/passwd suggests data theft preparation.

A critical lesson from this scenario is that AI security is not only about model accuracy. Organisations must verify whether models are:

* **Safe to load**  
* **Safe to execute**  
* **Safe to trust**  
* **Safe to integrate into production**

Modern AI defence strategies should include:

* **Sandboxed Evaluation**  
* **Telemetry Monitoring**  
* **Checksum Verification**  
* **Static Analysis Tools**  
* **Dependency Auditing**  
* **Behavioural Testing**  
* **Prompt Governance**  
* **Strict Production Approval Gates**

Without these controls, attackers can compromise systems through hidden AI supply chain vectors that remain undetected for weeks or months.

---

## **Candidate A Suspicious File Access**

Answer: /etc/passwd

## **Disabled Security Guardrail**

Answer: security\_review\_flag

## **Policy Template Governing Candidate A**

Answer: CommunityReview

## **Flag Retrieved From the Investigation**

Answer: THM{supp1y\_ch41n\_0wn3d}

## **Production Recommendation for Candidate A**

Answer: Reject

## **Approved Candidate for Production Deployment**

Answer: B

