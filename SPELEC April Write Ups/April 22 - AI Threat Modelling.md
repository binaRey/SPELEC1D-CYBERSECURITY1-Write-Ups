## **Brief Overview: AI Assets and Threat Modeling**

Traditional security focuses on assets like **databases**, **API keys**, and **credentials**, but AI systems introduce entirely new assets that require different protection strategies. These AI-specific assets become major attack targets because compromising them can alter how the AI behaves, what it knows, or how it makes decisions.

Unlike traditional systems, AI models are also:

* **Non-deterministic** → same input may produce different outputs  
* **Black boxes** → difficult to fully explain or trace internally

This changes how defenders perform **threat modeling**, because security teams must now protect not only infrastructure, but also the AI’s:

* Learning process  
* Internal representations  
* Behavioral rules  
* Decision-making mechanisms

## **Key Words / Important Terms to Address**

* **Threat Modeling**  
* **AI Asset Map**  
* **Training Data**  
* **Model Weights / Parameters**  
* **Embedding Vectors**  
* **System Prompts**  
* **Feature Stores**  
* **Model Registry**  
* **Artifacts**  
* **RAG Pipelines**  
* **Recommendation Engines**  
* **Fraud Detection**  
* **Data Poisoning**  
* **Backdoored Models**  
* **Model Exfiltration**  
* **Behavioral Constraints**  
* **Guardrails**  
* **Non-Deterministic Behaviour**  
* **Black Box Problem**  
* **Explainability**  
* **Inference Time**  
* **Deep Neural Networks**  
* **Input-Output Behaviour**  
* **Failure Modes**  
* **AI Security**  
* **Model Integrity**  
* **Attack Surface**

## **Important AI Assets**

### **Training Data**

The data used to train the AI model.

### **Risk:**

* **Data Poisoning**  
* Corrupted outputs  
* Hidden malicious behavior

### **Why Important:**

Damage becomes embedded into the model itself.

### **Model Weights / Parameters**

The numerical values representing what the model learned.

### **Risk:**

* **Model Theft**  
* Intellectual property loss  
* Functional AI duplication

### **Why Important:**

Stealing weights means stealing the AI itself.

### **Embedding Vectors**

Numerical representations used for:

* Similarity search  
* RAG systems  
* Recommendations

### **Risk:**

* Embedding manipulation  
* Retrieval poisoning  
* Altered AI context

### **System Prompts**

Hidden instructions controlling AI behavior.

### **Risk:**

* **Prompt Leakage**  
* Security bypass  
* Exposure of guardrails

### **Why Important:**

Reveals internal rules and security logic.

### **Feature Stores**

Preprocessed data feeding real-time inference.

### **Risk:**

* Tampered model inputs  
* Misleading predictions

### **Model Registry / Artifacts**

Storage for deployable AI models.

### **Risk:**

* Backdoored models  
* Malicious model replacement

### **Why Important:**

Attackers can silently replace legitimate models.

## **Key AI Characteristics**

### **Non-Deterministic Behaviour**

The same prompt can generate different outputs.

### **Security Impact:**

* Harder debugging  
* Difficult incident reproduction  
* Complex auditing

### **Black Box Problem**

AI decision-making is often not fully explainable.

### **Security Impact:**

* Hard to inspect reasoning  
* Defenders rely on:  
  * Input-output analysis  
  * Behaviour monitoring  
  * Failure pattern detection

---

In a RAG-based system, which AI asset type is used to retrieve relevant context at query time?

Answer: **Embedding Vectors**

Question: An attacker gains access to MegaCorp's model registry and swaps the production model for a modified version. Which AI-specific asset has been compromised?

Answer: **Model Registry / Artifacts**

---

## **Brief Overview: AI Data Supply Chain & STRIDE Limitations**

AI systems introduce a new type of **supply chain risk** centered around **data**, not just software dependencies. Every stage of the AI lifecycle — from **data collection** to **model inference** — creates opportunities for attackers to manipulate, poison, or replace components of the system.

Unlike traditional software attacks, AI compromises can remain hidden for weeks or months because poisoned data becomes embedded inside the model itself.

## **Key Words / Important Terms to Address**

* **AI Data Supply Chain**  
* **Training Pipeline**  
* **Data Poisoning**  
* **Decision Boundaries**  
* **Model Weights**  
* **Backdoored Models**  
* **Model Registry**  
* **Inference Stage**  
* **Vector Databases**  
* **RAG Pipelines**  
* **Injection Points**  
* **Validation Dataset**  
* **Trigger Inputs**  
* **Supply Chain Threats**  
* **Training Data Integrity**  
* **Adversarial Manipulation**  
* **Hallucinations**  
* **Jailbreaking**  
* **Elevation of Privilege**  
* **Model Theft**  
* **Information Disclosure**  
* **STRIDE**  
* **Tampering**  
* **Spoofing**  
* **Denial of Service**  
* **AI Threat Modeling**  
* **MITRE ATLAS**  
* **AI Security Frameworks**

## **AI Data Supply Chain Stages**

### **1\. Data Collection**

Data gathered from:

* Web scraping  
* Internal databases  
* Third-party providers  
* User-generated content

### **Main Risk:**

* Attackers injecting malicious data early in the pipeline

### **2\. Cleaning and Labelling**

Data is:

* Filtered  
* Processed  
* Labeled

### **Main Risk:**

* Mislabelled data teaches incorrect patterns  
* Hidden corruption inside datasets

### **3\. Model Training**

The model learns from prepared datasets.

### **Main Risk:**

* Poisoned data becomes embedded into:  
  * Model behavior  
  * Model weights  
  * Decision-making logic

### **Important:**

Retraining may be required to fully remove poisoning.

### **4\. Validation and Packaging**

The model is:

* Tested  
* Versioned  
* Stored in registries

### **Main Risk:**

* Backdoored model replacement  
* Malicious model swapping

### **5\. Inference**

The model serves predictions in production.

### **Main Risk:**

* RAG pipeline injection  
* Context manipulation  
* External data poisoning

## **Why STRIDE Alone Is Not Enough**

Traditional STRIDE categories struggle with AI threats because AI attacks are:

* Delayed  
* Diffuse  
* Difficult to detect  
* Context-dependent

### **Main Limitations:**

### **Tampering**

Training data poisoning is more subtle than traditional data modification.

### **Elevation of Privilege**

AI tools and autonomous actions expand what “privilege” means.

### **Information Disclosure**

Stealing model weights is far more damaging than stealing normal data.

### **Spoofing / Tampering Overlap**

Prompt injection and adversarial inputs blur multiple categories at once.

## **Main Takeaway**

AI systems require:

* New threat models  
* AI-aware STRIDE adaptations  
* Continuous monitoring of the full data lifecycle

🔑 Key Insight:  
 In AI security, compromising the training pipeline can silently alter the model’s future behavior long before anyone notices.

---

Question: An attacker injects crafted data points into a training pipeline over several months, gradually shifting the model's decision boundaries. At which supply chain stage does the attacker inject the malicious data?

Answer: **Data Collection**

Question: Which STRIDE category is insufficient for capturing the delayed, diffuse effects of training data poisoning?

Answer: **Tampering**

---

## **Brief Overview: STRIDE-AI Threat Mapping**

Traditional **STRIDE** still works for AI systems, but it must be adapted because AI introduces:

* New attack surfaces  
* Context-driven behavior  
* Autonomous actions  
* Expensive inference operations

Each STRIDE category now has AI-specific manifestations such as:

* **Data Poisoning**  
* **Prompt Injection**  
* **Model Extraction**  
* **Denial of Wallet**  
* **Jailbreaking**  
* **RAG Injection**

AI threats often blur multiple STRIDE categories at once, making AI security more complex than traditional applications.

## **Key Words / Important Terms to Address**

* **STRIDE-AI**  
* **Spoofing**  
* **Tampering**  
* **Repudiation**  
* **Information Disclosure**  
* **Denial of Service**  
* **Elevation of Privilege**  
* **Data Source Impersonation**  
* **RAG Injection**  
* **Data Poisoning**  
* **Prompt Injection**  
* **Model Manipulation**  
* **Feature Tampering**  
* **Decision Audit Trails**  
* **Model Extraction**  
* **Model Stealing**  
* **Training Data Extraction**  
* **Prompt Leakage**  
* **Embedding Inversion**  
* **Inference Cost Exploitation**  
* **Denial of Wallet (DoW)**  
* **GPU Exhaustion**  
* **Jailbreaking**  
* **Guardrail Bypass**  
* **Excessive Agency**  
* **Tool Exploitation**  
* **Cross-Plugin Escalation**  
* **Adversarial Examples**  
* **Emergent Behaviours**  
* **Model Bias**  
* **MITRE ATLAS**  
* **OWASP LLM Top 10**

## **STRIDE-AI Mapping Summary**

### **Spoofing**

AI Version:

* **Data Source Impersonation**  
* Fake RAG knowledge sources

### **Risk:**

The AI trusts attacker-controlled information.

### **Tampering**

AI Version:

* **Data Poisoning**  
* Prompt injection  
* Feature manipulation

### **Risk:**

The model learns or behaves incorrectly.

### **Repudiation**

AI Version:

* Missing AI decision audit trails

### **Risk:**

Impossible to explain:

* Why the AI made a decision  
* Which model version acted

### **Information Disclosure**

AI Version:

* **Model Extraction / Model Stealing**

### **Risk:**

Attackers reconstruct or steal AI capabilities.

### **Denial of Service**

AI Version:

* **Inference Cost Exploitation**  
* **Denial of Wallet**

### **Risk:**

Financial exhaustion without system downtime.

### **Elevation of Privilege**

AI Version:

* **Jailbreaking**  
* **Guardrail Bypass**  
* Excessive agency

### **Risk:**

Attackers gain unauthorized AI capabilities.

## **What STRIDE Still Misses**

Some AI threats do not fit neatly into STRIDE:

### **Adversarial Examples**

Can involve:

* Tampering  
* Spoofing  
* Privilege escalation

### **Model Bias & Fairness**

Security-adjacent but not traditional attacks.

### **Emergent Behaviours**

Unexpected AI capabilities no one predicted.

## **Main Takeaway**

AI systems require:

* Traditional STRIDE  
* AI-specific adaptations  
* Supplementary frameworks like:  
  * **MITRE ATLAS**  
  * **OWASP LLM Top 10**

🔑 AI security is not only about protecting systems — it is about controlling:

* AI behavior  
* Model permissions  
* Data integrity  
* Operational costs  
* Human trust

---

Question: What is the primary AI-specific manifestation of Information Disclosure in the STRIDE-AI mapping?

Answer: **Model Extraction**

Question: An attacker crafts prompts that cause an LLM to bypass its safety guidelines and content restrictions. Which STRIDE category does this map to?

Answer: **Elevation of Privilege**

Question: Which OWASP LLM Top 10 (2025) entry addresses the risks of AI systems being granted too many permissions or too much autonomy?

Answer: **LLM06: 2025 -Excessive Agency**

Question: An attacker drives your monthly inference bill from $15,000 to $180,000 without taking your service offline. What is this type of attack commonly called?

Answer: **Denial of Wallet**

---

## **Brief Overview: MITRE ATLAS for AI Threat Modeling**

**MITRE ATLAS** is the AI-focused counterpart of the traditional **MITRE ATT\&CK** framework. It helps defenders understand:

* How attackers target AI systems  
* Which techniques they use  
* What mitigations can stop them

ATLAS fills the gaps left by **STRIDE** by providing:

* AI-specific attack techniques  
* Real-world case studies  
* Tactical adversary behavior  
* Defensive countermeasures

It is especially useful for:

* **LLM security**  
* **AI supply chain attacks**  
* **Prompt injection**  
* **Data poisoning**  
* **Model theft**  
* **Adversarial ML attacks**

## **Key Words / Important Terms to Address**

* **MITRE ATLAS**  
* **MITRE ATT\&CK**  
* **Adversarial Threat Landscape for Artificial-Intelligence Systems**  
* **Tactics**  
* **Techniques**  
* **Sub-techniques**  
* **Mitigations**  
* **Case Studies**  
* **Data Poisoning**  
* **Model Extraction**  
* **Prompt Injection**  
* **Backdoor ML Model**  
* **Evade ML Model**  
* **RAG Injection**  
* **Training Pipeline**  
* **Adversarial Data**  
* **AI Supply Chain**  
* **ShadowRay**  
* **Morris II Worm**  
* **AML.T0020**  
* **AML.T0024**  
* **AML.T0051**  
* **AML.T0018**  
* **AML.T0015**  
* **Threat Modeling**  
* **Defensive Playbook**  
* **AI Infrastructure**  
* **PII Extraction**  
* **Self-Replicating Worm**

## **Important ATLAS Techniques**

### **Data Poisoning — AML.T0020**

Attackers inject malicious data into training datasets.

### **Impact:**

* Corrupts model behavior  
* Persistent until retraining

### **Maps to:**

* **STRIDE: Tampering**

### **Model Extraction — AML.T0024**

Attackers copy model behavior through API queries.

### **Impact:**

* AI intellectual property theft  
* Surrogate/shadow model creation

### **Maps to:**

* **STRIDE: Information Disclosure**

### **Evade ML Model — AML.T0015**

Adversarial inputs force incorrect predictions.

### **Impact:**

* Misclassification  
* Malware/filter bypass

### **Maps to:**

* Tampering  
* Spoofing  
* Elevation of Privilege

### **LLM Prompt Injection — AML.T0051**

Attackers manipulate LLM behavior through malicious prompts.

### **Types:**

* Direct Injection  
* Indirect Injection (RAG/document-based)

### **Maps to:**

* **STRIDE: Tampering**

### **Backdoor ML Model — AML.T0018**

Hidden triggers are embedded during training.

### **Impact:**

* Malicious behavior activates only with trigger patterns

## **Real-World Case Studies**

### **ShadowRay — AML.CS0023**

Attackers compromised distributed AI training systems using vulnerabilities in the **Ray** framework.

### **Importance:**

Shows that AI infrastructure attacks are already happening in production environments.

### **Morris II Worm — AML.CS0024**

A self-replicating AI prompt injection worm that spread through:

* AI agents  
* RAG email systems

### **Capabilities:**

* Prompt injection  
* PII extraction  
* Autonomous propagation

## **Main Takeaway**

🔑 STRIDE identifies the threat category.  
 🔑 MITRE ATLAS explains how the attack works.  
 🔑 Together, they create a complete AI threat modeling workflow.

ATLAS provides:

* Technical attack details  
* Real-world examples  
* AI-specific mitigations  
* Standardized technique IDs

---

Question: What does the acronym ATLAS stand for?

Answer: **Adversarial Threat Landscape for Artificial-Intelligence Systems**

Question: Which ATLAS case study described a self-replicating prompt injection worm that spread between AI agents via RAG email systems?

Answer: **Morris II**

Question: What is the ATLAS technique ID for Model Extraction?

Answer: **AML.T0024**

---

## **Brief Overview: OWASP LLM Top 10 for AI Architectures**

The **OWASP LLM Top 10 (2025)** is a security framework specifically designed for **LLM applications**. Unlike traditional OWASP, this version focuses on:

* AI-specific risks  
* LLM deployment weaknesses  
* RAG vulnerabilities  
* Prompt attacks  
* Model misuse

Its biggest advantage is **component mapping** — identifying:

* Which AI component is vulnerable  
* What threat affects it  
* Where security controls should be applied

This makes it useful for:

* Threat modeling  
* AI architecture reviews  
* Security hardening  
* Risk prioritization

---

## **Key Words / Important Terms to Address**

* **OWASP LLM Top 10**  
* **LLM Inference Endpoint**  
* **Prompt Injection**  
* **Sensitive Information Disclosure**  
* **Supply Chain Risks**  
* **Data Poisoning**  
* **Improper Output Handling**  
* **Excessive Agency**  
* **System Prompt Leakage**  
* **Vector Database**  
* **Embedding Weaknesses**  
* **Misinformation**  
* **Unbounded Consumption**  
* **RAG Pipeline**  
* **Training Pipeline**  
* **Model Registry**  
* **Output Sanitisation**  
* **XSS Risk**  
* **Rate Limiting**  
* **Denial of Wallet**  
* **Plugin Integrations**  
* **Agentic Orchestration**  
* **Embedding Poisoning**  
* **Hallucination**  
* **API Gateway**  
* **Third-Party Dependencies**

---

## **Important OWASP Risk Areas**

### **LLM01 — Prompt Injection**

Attackers manipulate model behavior through:

* Direct prompts  
* Indirect RAG content

### **Vulnerable Components:**

* LLM endpoint  
* Vector database  
* RAG pipeline

---

### **LLM03 — Supply Chain**

Compromised:

* Models  
* Datasets  
* Plugins  
* Dependencies

### **Main Target:**

* **Training Pipeline**

---

### **LLM05 — Improper Output Handling**

Unsafe LLM output reaches:

* Browsers  
* APIs  
* Databases

### **Risk:**

* XSS  
* Injection attacks  
* Unsafe rendering

---

### **LLM06 — Excessive Agency**

AI receives:

* Too many permissions  
* Autonomous tools  
* Dangerous access rights

### **Risk:**

* Database abuse  
* Unauthorized actions  
* Tool exploitation

---

### **LLM08 — Vector & Embedding Weaknesses**

Targets:

* Vector databases  
* Embedding systems  
* RAG retrieval

### **Risk:**

* Retrieval manipulation  
* Embedding poisoning  
* Unauthorized data access

---

### **LLM10 — Unbounded Consumption**

Attackers abuse:

* Expensive prompts  
* Massive token usage  
* Resource-heavy requests

### **Result:**

* Denial of Wallet (DoW)  
* Financial exhaustion  
* Service degradation

---

## **Main Takeaway**

🔑 OWASP helps defenders identify:

* Which AI component is vulnerable  
* Which risk affects it  
* Where security hardening is needed

### **Framework Relationship:**

* **STRIDE-AI** → identifies threat type  
* **MITRE ATLAS** → explains attack technique  
* **OWASP LLM Top 10** → maps risk to architecture components

Together, they form a complete AI threat modeling strategy.

---

Question: How many of the OWASP LLM Top 10 entries affect the LLM Inference Endpoint?

Answer: **7**

Question: An organisation notices their chatbot is rendering LLM output directly in the browser without sanitisation. Which OWASP entry does this fall under?

Answer: **Improper Output Handling**

Question: Which component in a typical LLM architecture is the primary one that needs hardening against data and model supply chain risks (LLM03)?

Answer: **Training Pipeline**

---

**Practical:**  
Click the green **View Site** button to open the **AI** Threat Modelling exercise: you'll be selecting **OWASP** **LLM** Top 10 vulnerabilities, mapping them to architecture components, and justifying your choices to put your threat modelling instincts to the test. This task can be used to practice your knowledge of **AI** systems and threats. Good luck\!   
**FLAG: What's the flag?**   
**THM{AI\_THREAT\_MODEL\_COMPLETE}**   
---

## **Brief Overview: Complete AI Threat Modeling Workflow**

This room covered the full process of **AI threat modeling** using three major frameworks together:

* **STRIDE-AI**  
* **MITRE ATLAS**  
* **OWASP LLM Top 10**

The goal is to identify:

* AI-specific assets  
* Attack surfaces  
* Supply chain risks  
* LLM vulnerabilities  
* Component-level weaknesses

The workflow is designed to be **repeatable** for:

* New AI deployments  
* Model updates  
* Agentic AI systems  
* RAG pipelines  
* Enterprise AI architectures

## **Key Words / Important Terms to Address**

* **AI Threat Modeling**  
* **Training Data**  
* **Model Weights**  
* **Embeddings**  
* **System Prompts**  
* **Feature Stores**  
* **Model Registries**  
* **AI Data Supply Chain**  
* **Inference**  
* **Data Poisoning**  
* **Jailbreaking**  
* **Prompt Injection**  
* **Elevation of Privilege**  
* **STRIDE-AI**  
* **MITRE ATLAS**  
* **OWASP LLM Top 10**  
* **Threat Assessment**  
* **RAG Pipeline**  
* **Agentic AI**  
* **AI Architecture**  
* **Attack Surface**  
* **Model Updates**  
* **Risk Prioritisation**  
* **Technique IDs**  
* **Mitigations**  
* **Component Mapping**  
* **AI Security Controls**  
* **AI Governance**  
* **Inference Endpoint**

## **AI Threat Modeling Workflow Summary**

### **1\. Identify AI-Specific Assets**

Understand what needs protection:

* Training datasets  
* Model weights  
* Embeddings  
* System prompts  
* Feature stores  
* Model registries

### **Why Important:**

These assets create entirely new attack surfaces.

### **2\. Map the AI Data Supply Chain**

Track data flow from:

* Collection  
* Cleaning  
* Training  
* Validation  
* Deployment  
* Inference

### **Main Risk:**

Compromise at any stage affects the entire AI lifecycle.

### **3\. Adapt STRIDE for AI**

Apply traditional STRIDE categories in AI context.

Examples:

* **Tampering → Data Poisoning**  
* **Elevation of Privilege → Jailbreaking**  
* **Information Disclosure → Model Extraction**

### **Purpose:**

Categorize AI threats systematically.

### **4\. Enrich Findings with MITRE ATLAS**

Use:

* Technique IDs  
* Real-world AI attack methods  
* AI-specific mitigations

### **Purpose:**

Understand how attacks actually happen.

### **5\. Use OWASP LLM Top 10**

Map risks directly to architecture components.

Examples:

* Prompt Injection → Inference Endpoint  
* Supply Chain → Training Pipeline  
* Embedding Weaknesses → Vector Database

### **Purpose:**

Prioritize security hardening.

### **6\. Generate a Threat Assessment**

Combine:

* STRIDE categories  
* ATLAS techniques  
* OWASP component mapping

### **Result:**

A practical, prioritized AI security assessment.

## **Main Takeaways**

### **AI Systems Are Different**

AI introduces:

* New assets  
* New supply chains  
* Non-deterministic behavior  
* Emergent risks

Traditional security alone is not enough.

### **STRIDE \+ ATLAS \+ OWASP Work Together**

| Framework | Purpose |
| ----- | ----- |
| STRIDE-AI | Identifies threat category |
| MITRE ATLAS | Explains attack techniques |
| OWASP LLM Top 10 | Maps risk to components |

### **OWASP Is the Operational Lens**

OWASP helps defenders identify:

* Which component is vulnerable  
* What risk exists  
* Which controls are needed

This makes AI threat modeling actionable in real deployments.

## **Final Key Insight**

🔑 Effective AI security requires:

* Threat modeling  
* Architectural analysis  
* Continuous monitoring  
* AI-specific mitigations  
* Component-level hardening

AI systems are not just software applications — they are dynamic systems with evolving behaviors, data dependencies, and autonomous capabilities that require dedicated security frameworks.

