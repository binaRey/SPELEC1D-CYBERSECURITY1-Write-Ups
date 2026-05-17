## **Brief Overview**

### **A Roll of the Dice: Understanding AI Defence-in-Depth**

Prompt injection and jailbreaking attacks work because **Large Language Models (LLMs)** do not follow hard-coded security rules. Instead, they generate responses based on **probability distributions** learned during training. When attackers manipulate prompts, they are not exploiting a traditional software bug; they are influencing the likelihood that the model will produce a harmful or compliant response.

This makes AI security fundamentally different from traditional cybersecurity. There is no single vulnerability to patch, no perfect filter, and no guaranteed way to stop prompt injection completely. Even companies such as OpenAI acknowledge that prompt injection may never be fully solved within current AI architectures.

A common misunderstanding is that stronger **system prompts** alone can fully secure a model. Although system prompts help guide behaviour, they exist within the same natural language context as user input and external content. Since the model processes all text probabilistically, malicious prompts can sometimes outweigh or override system instructions.

Because no single defence is reliable, AI security relies on **Defence-in-Depth**, a layered security strategy where multiple controls work together. If one protection layer fails, additional safeguards remain in place to reduce risk and limit damage.

Key defence layers include:

* **System Prompt Hardening** – Strengthening instruction design and safety guidance.  
* **Input Guardrails** – Detecting malicious prompts before they reach the model.  
* **Deployment Controls** – Restricting what AI systems are allowed to access or execute.  
* **Output Validation** – Treating AI-generated responses as untrusted until verified.

Defence-in-depth reduces the likelihood of successful attacks by:

* Increasing attacker effort  
* Reducing the blast radius of compromises  
* Improving detection of malicious behaviour

This layered approach is considered the most practical strategy for protecting AI systems against prompt injection and jailbreaking attacks.

Key concepts that need attention include:

* **Defence-in-Depth**  
* **Prompt Injection Mitigation**  
* **Jailbreaking Defence**  
* **System Prompt Hardening**  
* **Input Guardrails**  
* **Output Validation**  
* **Probabilistic Security**  
* **LLM Security Architecture**  
* **Blast Radius Reduction**  
* **Layered Security Controls**  
* **AI Risk Mitigation**  
* **Security Monitoring**

Understanding defence-in-depth is important because it explains why AI security requires multiple overlapping protections instead of relying on a single safeguard or perfect solution.

## **Assessment**

1. What term describes the security philosophy of stacking multiple controls so that breaking one still leaves an attacker facing others?

Answer: **Defence-in-Depth**

## **Brief Overview**

### **The First Line of Defence**

The **System Prompt** is one of the most important security layers in a **Large Language Model (LLM)** deployment. It defines the model’s role, restrictions, behaviour, and boundaries before any user interaction occurs. Since attackers commonly target system prompts during **Prompt Injection** and **Jailbreaking** attacks, developers must harden them carefully to reduce exploitation risks.

One key defence technique is **Tight Scoping**, where the model is restricted to a very specific purpose. For example, a support chatbot may only answer billing-related questions and refuse unrelated requests. Narrowing the model’s responsibilities reduces opportunities for attackers to manipulate its behaviour.

Another important technique is using **Explicit Refusal Instructions**. These prompts clearly tell the AI how to react when users attempt to override instructions, reveal system prompts, or bypass restrictions. Although not perfect, these instructions increase the difficulty of successful attacks.

A third technique is **Persona Restriction**, which blocks the model from adopting fictional characters, alternative personalities, or roleplay scenarios that conflict with its intended purpose. This directly targets roleplay-based jailbreaks such as the **DAN Prompt** and the **Grandma Exploit**.

Developers must also avoid storing sensitive information inside system prompts. According to OWASP guidance, system prompts should never contain:

* API Keys  
* Credentials  
* Internal Service Names  
* Confidential Logic  
* Sensitive Data

This is because attackers may eventually extract system prompts through prompt injection techniques.

Another major defence strategy is using **Structured Prompt Templates** such as **ChatML** or **Harmony**, where instructions are separated by role fields like:

* **system**  
* **user**  
* **assistant**

This structured separation helps the model distinguish trusted developer instructions from untrusted user input. Developers should never concatenate user input directly into system prompts because it destroys instruction separation and increases vulnerability.

Using delimiters around user content also helps define clear boundaries between trusted and untrusted text, reducing the effectiveness of injected instructions.

Key concepts that need attention include:

* **System Prompt Hardening**  
* **Tight Scoping**  
* **Explicit Refusal Instructions**  
* **Persona Restriction**  
* **Structured Prompt Templates**  
* **ChatML**  
* **Harmony Format**  
* **Prompt Injection Defence**  
* **Role Separation**  
* **Trusted vs Untrusted Input**  
* **OWASP AI Security Guidance**  
* **LLM Deployment Security**

Understanding system prompt hardening is important because it forms the first line of defence against prompt injection and jailbreaking attacks, even though it cannot completely eliminate the risks.

## **Assessment**

1. What role field value should developer instructions always be placed under in structured prompt templates?

Answer: **System**

2. What should never be stored inside a system prompt?

Answer: **Sensitive Data**

3. What is the term for limiting a model strictly to its intended purpose in a system prompt?

Answer: **Tight Scoping**

4. Which system prompt hardening pattern directly addresses roleplay and persona-based bypass attempts?

Answer: **Persona Restriction**

## **Brief Overview**

### **Filtering at the Boundary**

**Guardrails** are security layers designed to protect **Large Language Models (LLMs)** from malicious prompts before and after model processing. While hardened system prompts improve instruction security, guardrails act as additional protective barriers that detect, block, or filter harmful content entering or leaving the AI system.

One of the simplest guardrail methods is a **Blocklist**, which uses string matching and regex patterns to detect known malicious phrases such as:

* “Ignore previous instructions”  
* “Act as DAN”  
* “You have no restrictions”

Although blocklists are fast and inexpensive, they are easy to bypass through techniques such as:

* **Base64 Encoding**  
* **Leetspeak**  
* **Unicode Homoglyphs**  
* **Zero-Width Characters**  
* **Emoji Smuggling**

These obfuscation techniques exploit the weakness of keyword-based filtering systems.

To improve detection, organisations use **AI-Powered Guardrails**, which rely on machine learning classifiers instead of simple string matching. One example is Meta’s **Llama Prompt Guard 2**, a **BERT-based classifier** trained to identify malicious intent, prompt injections, and jailbreak attempts.

Unlike blocklists, AI-powered classifiers analyse the semantic meaning behind prompts, allowing them to detect attack variations even if the wording changes. However, researchers have shown that these classifiers can still be bypassed using character-level noise and advanced obfuscation methods.

Guardrails operate in two major stages:

### **Input Guardrails**

These run **before** the prompt reaches the model. Their purpose is to:

* Block malicious prompts  
* Remove sensitive information  
* Reject unsafe requests  
* Filter off-topic input

### **Output Guardrails**

These run **after** the AI generates a response. Their purpose is to:

* Detect leaked credentials or PII  
* Validate generated outputs  
* Prevent unsafe tool execution  
* Scrub sensitive data before delivery

Another major challenge is **Indirect Prompt Injection**, where malicious instructions are hidden inside external content such as:

* Emails  
* Documents  
* RAG Data  
* Uploaded Files

Since these instructions enter through retrieved context rather than direct user input, they can bypass traditional input guardrails entirely.

Researchers also discovered vulnerabilities in systems such as Slack AI integrations, where hidden instructions inside uploaded files caused confidential data leakage from private channels.

Guardrails also involve trade-offs between:

* **Latency**  
* **Accuracy**  
* **User Experience**  
* **Detection Coverage**

Heavier AI-based filtering systems provide stronger security but may increase response delays or falsely block legitimate requests.

Key concepts that need attention include:

* **Guardrails**  
* **Input Guardrails**  
* **Output Guardrails**  
* **Blocklists**  
* **Regex Filtering**  
* **AI-Powered Classifiers**  
* **Llama Prompt Guard 2**  
* **BERT-Based Security Models**  
* **Indirect Prompt Injection**  
* **Unicode Obfuscation**  
* **PII Filtering**  
* **AI Security Trade-Offs**

Understanding guardrails is important because they provide critical layered protection against prompt injection and jailbreaking attacks, even though they cannot completely eliminate all AI security risks.

## **Assessment**

1. What type of guardrail uses string matching and regex patterns to reject requests based on known attack phrases?

Answer: **Blocklist**

2. What type of guardrail runs before the model receives the user's prompt?

Answer: **Input Guardrail**

3. What BERT-based classifier developed by Meta is used as an AI-powered input guardrail?

Answer: **Llama Prompt Guard 2**

## **Brief Overview**

### **Deployment Controls and Output Security**

Even with strong **Guardrails** and hardened **System Prompts**, some prompt injection and jailbreaking attacks will still succeed. This is why **Deployment Controls** are essential. Instead of only preventing attacks, deployment security focuses on limiting what an AI system can actually access or do after compromise.

A core security principle used here is the **Principle of Least Privilege**, which states that every component should only have the minimum permissions required to perform its task. In AI systems, this greatly reduces the **blast radius** of successful prompt injections.

For example, attacks such as the **Slack AI Vulnerability**, **EchoLeak**, and **Cursor Remote Code Execution (RCE)** became dangerous because the AI systems had excessive access to:

* Private channels  
* Internal documents  
* Sensitive databases  
* External tools  
* File systems

If retrieval systems and permissions had been tightly scoped, the attackers would have gained far less valuable information even after successful injection.

This concept is especially important in **Retrieval-Augmented Generation (RAG)** systems. AI models should only retrieve documents and data that the current user is authorised to access. Proper architectural separation between:

* Trusted Instructions  
* External Content  
* User Permissions

helps prevent large-scale data exposure.

Another critical risk is **Improper Output Handling**, identified by OWASP as **LLM05:2025**. This vulnerability occurs when AI-generated output is directly trusted and executed by downstream systems without validation or sanitisation.

Examples include:

* **LLM-generated JavaScript** rendered in browsers causing **Cross-Site Scripting (XSS)**  
* **LLM-generated SQL** executed without parameterisation causing **SQL Injection (SQLi)**  
* **LLM-generated shell commands** executed directly leading to **Remote Code Execution (RCE)**

To prevent this, AI output must be treated with a **Zero-Trust Security Model**, just like untrusted user input. This involves:

* Output Validation  
* Content Sanitisation  
* Schema Enforcement  
* Safe Encoding  
* Strict Tool Call Validation

Additional defensive layers include:

* **Rate Limiting**  
* **Logging**  
* **Security Monitoring**  
* **Anomaly Detection**

These controls help detect suspicious behaviour, monitor potential data exfiltration, and provide audit trails when attacks occur.

Key concepts that need attention include:

* **Principle of Least Privilege**  
* **Deployment Controls**  
* **Blast Radius Reduction**  
* **Improper Output Handling**  
* **LLM05:2025**  
* **Output Guardrails**  
* **Zero-Trust Security**  
* **Cross-Site Scripting (XSS)**  
* **SQL Injection (SQLi)**  
* **Remote Code Execution (RCE)**  
* **RAG Security Scoping**  
* **Security Monitoring and Logging**

Understanding deployment controls is important because even if attackers successfully manipulate an AI model, properly designed permissions and output protections can significantly reduce the damage caused.

## **Assessment**

1. What foundational security principle states that every component should have only the permissions it needs to perform its job?

Answer: **Principle of Least Privilege**

2. What is the OWASP identifier for the vulnerability caused by unsanitised LLM output being passed to downstream systems?

Answer: **LLM05:2025**

3. What classic web vulnerability can result from LLM-generated JavaScript being rendered in a browser without sanitisation?

Answer: **XSS**

## **Bypassing Quadrails**

Can you get the flag?

Answer: THM{ja1lbre3ker}

## **Conclusion**

This room has established that while there is no silver bullet for attacks like **prompt injection**, there are measures you can take to ensure these attacks are significantly less likely to succeed through an in-depth, layered approach:

* **LLM security is probabilistic, not binary.** Attacks succeed by shifting probability distributions, which means defences must be layered rather than absolute.  
* **A hardened system prompt is the first line of defence**, using tight scoping, explicit refusal instructions, and structural separation from user input, but it cannot stand alone.  
* **Guardrails** filter malicious inputs before they reach the model and catch dangerous outputs before they leave, but can be bypassed through obfuscation and indirect injection.  
* **Deployment controls** limit what a compromised model can actually do, applying least privilege to tool access, data retrieval, and permissions to shrink the blast radius of a successful attack.  
* **Rate limiting, logging, and monitoring** ensure that when attacks slip through, they are detected, investigated, and used to strengthen future defences.

