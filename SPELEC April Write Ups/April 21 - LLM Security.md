### **Brief Overview: Data-Based Threats in LLMs**

**LLMs are data-driven systems**, which is both their greatest strength and a major security risk. Because models learn patterns from vast training datasets, attackers can sometimes **invert the learning process** and force the model to reveal information that should remain hidden. These are called **data-based threats**, and they primarily impact **privacy and confidentiality**.

The three major data-based threats are **Training Data Extraction**, **Membership Inference**, and **Prompt Leakage**. All involve **coaxing the model** into exposing hidden or sensitive data rather than generating new information.

## **Key Words / Critical Terms to Address**

* **Data-based threats**  
* **Training Data Extraction**  
* **Membership Inference**  
* **Prompt Leakage**  
* **Confidentiality**  
* **Privacy**  
* **Memorisation**  
* **Verbatim Output**  
* **PII (Personally Identifiable Information)**  
* **Secrets / Credentials**  
* **System Prompt**  
* **Developer Prompt**  
* **Prompt Injection**  
* **Conversation History**  
* **Hidden Instructions**  
* **White-box Access**  
* **Model Confidence**  
* **Likelihood / Perplexity**  
* **Attack Surface**  
* **Inverted Data Flow**  
* **Intellectual Property**  
* **Security Boundary**  
* **Privacy Metadata**

## **1\. Training Data Extraction**

**What it is:**  
 An attacker forces the model to **reproduce exact or near-exact snippets** from its training data.

**How it works:**

* Uses **crafted prompts**  
* Generates **large volumes of output**  
* Detects **memorisation signals**:  
  * High confidence  
  * Deterministic regeneration  
  * Realistic structured content

**Risk:**

* Leakage of **emails**, **SSH keys**, **private text**, or **PII**

**In a nutshell:**

* **Target:** Training dataset (**confidentiality**)  
* **Input:** Prompts designed to trigger memorised content  
* **Output:** Verbatim or near-verbatim training data

## **2\. Membership Inference**

**What it is:**  
 An attacker checks whether a **specific known data sample** was included in the model’s training set.

**Key distinction:**

* The attacker **already has the data**  
* The goal is **confirmation**, not extraction

**How it works:**

* Tests model behaviour on the sample  
* Looks for:  
  * Higher confidence  
  * Lower loss  
  * Familiar statistical patterns

**In a nutshell:**

* **Target:** Training dataset **membership** (privacy metadata)  
* **Input:** Known candidate data sample  
* **Output:** Yes/No or probability of inclusion

## **3\. Prompt Leakage (LLM07 – System Prompt Leakage)**

**What it is:**  
 The model reveals **hidden system or developer instructions** that define how it behaves.

**Why it matters:**

* System prompts often contain:  
  * **Business logic**  
  * **Safety rules**  
  * **Internal architecture hints**  
* Leakage exposes **intellectual property** and **security controls**

**How it happens:**

* A form of **prompt injection**  
* The model is tricked into:  
  * Repeating  
  * Summarising  
  * Reflecting on its instructions

**In a nutshell:**

* **Target:** System / developer prompt  
* **Input:** Prompts requesting instruction disclosure  
* **Output:** Partial or full prompt leakage  
* **Mitigation:**  
  * Never store **credentials**, **API keys**, or **secrets** in prompts  
  * Assume the prompt **can be extracted**

---

Which sample is a member?

Answer: **MI\_SAMPLE\_ALPHA**

Which attack determines whether a known data sample was part of an LLM’s training set?

Answer: **Membership Inference**

Which data-based threat involves the model reproducing memorised snippets of its training data?

Answer: **Training Data Extraction**

---

### **Brief Overview: Model-Based Threats in AI Systems**

**Unlike data-based threats, which target training data directly, model-based threats target the LLM itself by exploiting how information is stored inside the model’s parameters, weights, and internal representations.**

**The two major model-based threats are Model Extraction (Model Theft) and Model Inversion. These attacks can expose:**

* **Intellectual Property**  
* **Sensitive Training Data**  
* **Model Weights**  
* **Private Attributes**

**Both attacks abuse the model’s learned behavior to either replicate the model or recover hidden training information.**

# **Key Words / Important Terms to Address**

* **Model-Based Threats**  
* **Model Extraction**  
* **Model Theft**  
* **Model Inversion**  
* **Attack Surface**  
* **Model Parameters**  
* **Model Weights**  
* **Internal Representations**  
* **Surrogate Model**  
* **Distilled Model**  
* **API Queries**  
* **Input-Output Pairs**  
* **Decision Boundaries**  
* **Embeddings**  
* **Training Data Reconstruction**  
* **Privacy Breach**  
* **Intellectual Property**  
* **Memorised Data**  
* **LLM API**  
* **Reverse Engineering**  
* **Sensitive Attributes**  
* **Reconstruction Attack**  
* **Machine Learning Security**  
* **AI Privacy Risks**

# **1\. Model Extraction (Model Theft)**

### **What it is:**

**Attackers copy or replicate an AI model by sending large numbers of prompts through its API and collecting the responses.**

### **How it works:**

* **Attacker gathers:**  
  * **Input-output pairs**  
* **Uses collected data to train:**  
  * **Surrogate model**  
  * **Distilled model**  
* **Attempts to imitate:**  
  * **Model behavior**  
  * **Decision-making**  
  * **Possibly recover weights**

### **Main Risk:**

* **Theft of:**  
  * **Intellectual Property**  
  * **Expensive AI capabilities**  
  * **Proprietary business models**

### **Example:**

**Researchers recreated a smaller version of ChatGPT 3.5 Turbo using only about $50 in API costs.**

### **In a nutshell:**

* **Target: Model parameters / weights**  
* **Input: Massive API queries**  
* **Output: Replica or imitation model**

# **2\. Model Inversion**

### **What it is:**

**Attackers reconstruct sensitive training data by analyzing model responses and reversing the learning process.**

### **Key Difference from Membership Inference:**

| Membership Inference | Model Inversion |
| ----- | ----- |
| **Confirms if data existed in training** | **Reconstructs unknown data** |
| **Yes/No decision** | **Generates new recovered data** |
| **Uses known sample** | **Discovers hidden information** |

### **How it works:**

* **Queries model repeatedly**  
* **Optimizes prompts or embeddings**  
* **Reconstructs:**  
  * **Sensitive text**  
  * **Personal attributes**  
  * **Hidden records**

### **Main Risk:**

* **Privacy breaches**  
* **Leakage of memorized confidential information**

### **Example:**

**Researchers extracted verbatim chunks of ChatGPT training data in 2023\.**

### **In a nutshell:**

* **Target: Internal model representations**  
* **Input: Model outputs / embeddings**  
* **Output: Reconstructed training data**

# **Core Security Insight**

**LLMs do not only process data — they can also store patterns and information internally.**  
 **This makes the model itself an attack surface.**

---

**What is the employee ID?**

**Answer: 7814**

**Which model-based threat attempts to reconstruct sensitive information encoded within a model’s internal representations?**

**Answer: Model Inversion**

---

### **Brief Overview: System-Integration Threats in LLMs**

**LLMs introduce unique security risks because they process system instructions, user prompts, and external content together inside one shared context window without a strict security boundary. This means malicious user input can sometimes override trusted instructions, creating new attack vectors such as Prompt Injection, Context Overflow, and Memory Poisoning.**

**These attacks target the way LLMs handle context, memory, and token processing, rather than targeting databases or networks directly.**

# **Key Words / Important Terms to Address**

* **System-Integration Threats**  
* **Prompt Injection**  
* **Context Window**  
* **Context-Window Poisoning**  
* **Instruction Hierarchy**  
* **Trusted vs Untrusted Content**  
* **Token Limit Abuse**  
* **Context Overflow**  
* **LLM10 – Unbounded Consumption**  
* **Memory Poisoning**  
* **Persistent Conversation State**  
* **FIFO (First In, First Out)**  
* **Denial of Wallet (DoW)**  
* **Denial of Service (DoS)**  
* **Inference Costs**  
* **Token Budgets**  
* **Rate Limiting**  
* **Policy Bypass**  
* **Conversation History**  
* **Long-Term Context**  
* **Stateful Conversations**  
* **Malicious Prompts**  
* **Safeguard Truncation**  
* **Prompt Manipulation**  
* **AI Context Security**

# **1\. Prompt Injection**

### **What it is:**

**Attackers manipulate the LLM’s context so malicious instructions override trusted system prompts.**

### **Why it happens:**

**LLMs treat all tokens in the context window equally:**

* **System instructions**  
* **User prompts**  
* **Retrieved documents**

**There is no strong internal separation between trusted and untrusted content.**

### **Main Risk:**

* **Policy bypass**  
* **Harmful actions**  
* **Altered model behavior**

### **In a nutshell:**

* **Target: Context window / instruction hierarchy**  
* **Input: Malicious user or retrieved content**  
* **Output: Manipulated LLM behavior**

# **2\. Context Overflow (LLM10 – Unbounded Consumption)**

### **What it is:**

**Attackers overload the model with extremely large prompts or excessive content until important instructions are pushed out of memory.**

### **How it works:**

**LLM context windows operate like:**

* **FIFO (First In, First Out)**

**When the context becomes full:**

* **Older tokens are removed**  
* **Security instructions may disappear**  
* **Safeguards weaken**

### **Main Risks:**

* **Denial of Service (DoS)**  
* **Increased inference costs**  
* **Safeguard removal**  
* **Denial of Wallet (DoW)**

### **Mitigation:**

* **Rate limiting**  
* **Token budgets**  
* **Cost alerts**  
* **Context limits**

### **In a nutshell:**

* **Target: Context window size & resources**  
* **Input: Oversized prompts/documents**  
* **Output: Truncated safeguards or service disruption**

# **3\. Memory Poisoning**

### **What it is:**

**Attackers gradually inject false or malicious information into a chatbot’s persistent memory over multiple interactions.**

### **Why it’s dangerous:**

**The LLM remembers previous conversations and may later treat malicious information as trusted context.**

### **Main Risks:**

* **Persistent misinformation**  
* **Corrupted responses**  
* **Manipulated future behavior**

### **Example:**

**Convincing the model:**

* **“A cat is a dog”**

**Then later:**

* **The model gives incorrect answers based on poisoned memory.**

### **In a nutshell:**

* **Target: Persistent conversation memory**  
* **Input: Malicious long-term context**  
* **Output: Corrupted future responses**

---

**Did you convince the model? Whats the flag?**

**Answer: THM{MEMORY\_POISONED}** 

**Which system component combines system instructions, retrieved data, and user input into a single sequence?**

**Answer: The Context Window**

---

### **Brief Overview: Human-Focused Threats Using LLMs**

**LLMs are not only a new attack surface, but also a powerful tool attackers can use against people. AI can generate highly convincing scams, phishing emails, fake information, and misleading recommendations that exploit human trust and decision-making.**

**The two major human-focused threats discussed are LLM-Powered Social Engineering and Trust Exploitation (LLM09 – Misinformation). These attacks succeed by making users trust AI-generated content without proper verification.**

# **Key Words / Important Terms to Address**

* **LLM-Powered Social Engineering**  
* **AI Phishing**  
* **Spear Phishing**  
* **Trust Exploitation**  
* **LLM09 – Misinformation**  
* **Hallucination**  
* **Human Cognition**  
* **Decision-Making**  
* **OSINT (Open Source Intelligence)**  
* **Manipulated Users**  
* **Fraud**  
* **Coerced Actions**  
* **Fake Packages**  
* **Package Hallucination**  
* **Malware Distribution**  
* **Social Engineering**  
* **AI-Generated Scams**  
* **Human Trust**  
* **Authoritative Responses**  
* **Cyber Manipulation**  
* **Rogue Packages**  
* **False Information**  
* **Security Awareness**  
* **Verification**  
* **Attack Vector**  
* **Confidence Exploitation**

# **1\. LLM-Powered Social Engineering**

### **What it is:**

**Attackers use LLMs to create highly convincing phishing messages, scams, and impersonation attacks.**

### **Why it’s dangerous:**

**Traditional phishing clues are disappearing:**

* **Better grammar**  
* **More realistic tone**  
* **Personalized messaging**  
* **Executive impersonation**

### **Main Risk:**

**People trust the message because it appears authentic.**

### **Extra Threat:**

**If attackers gain access to:**

* **Internal company data**  
* **Customer information**  
* **OSINT**

**They can create extremely believable attacks.**

### **In a nutshell:**

* **Target: Human cognition & decision-making**  
* **Input: Personal/contextual information**  
* **Output: Successful phishing, fraud, manipulation**

# **2\. Trust Exploitation (LLM09 – Misinformation)**

### **What it is:**

**Users trust AI-generated answers even when they are incorrect, fabricated, or manipulated.**

### **Main Issue:**

**LLMs often sound:**

* **Confident**  
* **Professional**  
* **Authoritative**

**Even when the information is false (hallucinations).**

### **Example: Package Hallucination**

**An AI invents a fake software package:**

* **Example: `secure-utils-xtools`**

**Attackers then:**

* **Publish malware using that fake package name**  
* **Trick developers into installing it**

### **Why it matters:**

**Hallucinations become:**

* **An active attack vector**  
* **A method for malware delivery**

### **In a nutshell:**

* **Target: User trust & judgment**  
* **Input: Confident but false information**  
* **Output: Unsafe actions and harmful decisions**

---

Which package should you NOT download?

Answer: **robbco-llm-audit**

LLM-powered social engineering primarily amplifies which existing attack category?

Answer: **Phishing**

---

### **Brief Overview: A Secure LLM Mindset**

**LLMs introduce a completely new attack surface compared to traditional systems because they rely on natural language, shared context windows, and learned behavior. Security is no longer only about protecting servers or databases — it now includes protecting:**

* **Training data**  
* **Model behavior**  
* **Context handling**  
* **User trust**

**The major threat categories are:**

* **Data-Based Threats**  
* **Model-Based Threats**  
* **System-Based Threats**  
* **User-Based Threats**

**Together, these threats show how attackers can exploit both the AI system and the people using it.**

# **Key Words / Important Terms to Address**

* **Secure LLM Mindset**  
* **Attack Surface**  
* **Natural Language Interaction**  
* **Context Handling**  
* **Emergent Behaviour**  
* **Training Data Extraction**  
* **Membership Inference**  
* **Prompt Leakage**  
* **System Prompt Exposure**  
* **Model Extraction**  
* **Model Theft**  
* **Model Inversion**  
* **Prompt Injection**  
* **Context Window Poisoning**  
* **Context Overflow**  
* **Unbounded Consumption**  
* **Memory Poisoning**  
* **Stateful Conversations**  
* **LLM-Powered Social Engineering**  
* **Trust Exploitation**  
* **LLM09 – Misinformation**  
* **LLM07 – System Prompt Leakage**  
* **LLM10 – Unbounded Consumption**  
* **PII (Personally Identifiable Information)**  
* **Phishing**  
* **Policy Bypass**  
* **Denial of Service (DoS)**  
* **Human Cognition**  
* **User Trust**  
* **Security Awareness**  
* **AI Risk Management**

# **Main Threat Categories**

## **1\. Data-Based Threats**

**Attackers exploit what the model has learned from training data.**

### **Includes:**

* **Training Data Extraction**  
* **Membership Inference**  
* **Prompt Leakage**

### **Main Risks:**

* **Leakage of:**  
  * **PII**  
  * **Secrets**  
  * **Confidential training data**  
  * **Hidden prompts**

## **2\. Model-Based Threats**

**Attackers target the model itself.**

### **Includes:**

* **Model Extraction (Model Theft)**  
* **Model Inversion**

### **Main Risks:**

* **Intellectual property theft**  
* **Reconstructing sensitive training data**  
* **Copying model behavior**

## **3\. System-Based Threats**

**Attackers abuse how LLMs process context and memory.**

### **Includes:**

* **Prompt Injection**  
* **Context Overflow**  
* **Memory Poisoning**

### **Main Risks:**

* **Policy bypass**  
* **Corrupted responses**  
* **Denial of service**  
* **Persistent manipulation**

## **4\. User-Based Threats**

**Attackers use AI to manipulate people.**

### **Includes:**

* **AI-Powered Social Engineering**  
* **Trust Exploitation**  
* **Misinformation**

### **Main Risks:**

* **Phishing**  
* **Fraud**  
* **Malware installation**  
* **Unsafe decisions**

# **Key Takeaways**

### **LLMs create new security challenges because they:**

* **Process trusted and untrusted input together**  
* **Memorize patterns from training data**  
* **Maintain persistent context and memory**  
* **Generate convincing but sometimes false information**

### **Important Security Lessons:**

* **Never fully trust AI outputs**  
* **Validate prompts, outputs, and external tools**  
* **Apply layered security controls**  
* **Protect training data and prompts**  
* **Monitor model behavior continuously**  
* **Train users to verify AI-generated information**

