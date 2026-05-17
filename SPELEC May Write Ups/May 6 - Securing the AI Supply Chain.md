## **Brief Overview**

### **Safe Model Serialisation Defences**

The TryTrainMe breach demonstrated how dangerous Python **pickle** files can be in machine learning environments. The root issue comes from the **reduce** method, which allows arbitrary code execution during model loading.

To reduce this risk, modern ML security practices focus on replacing unsafe serialisation methods and restricting how models are loaded.

### **SafeTensors**

[SafeTensors](https://github.com/huggingface/safetensors?utm_source=chatgpt.com) was created by [Hugging Face](https://huggingface.co?utm_source=chatgpt.com) as a secure alternative to pickle-based formats.

Key advantages include:

* **No code execution**  
* **Validated file headers**  
* **Tensor-only storage**  
* **Faster loading**  
* **Zero-copy memory mapping**

Unlike pickle, SafeTensors cannot store executable Python instructions.

### **Pickle vs SafeTensors**

| Feature | Pickle | SafeTensors |
| ----- | ----- | ----- |
| Code execution | Yes | No |
| Stores Python objects | Yes | No |
| Stores tensor weights | Yes | Yes |
| Safe loading | No | Yes |

### **PyTorch Protection**

In PyTorch, using:

weights\_only=True

restricts pickle from executing arbitrary code and limits loading to tensor reconstruction only.

Example:

* Unsafe → torch.load("model.pt")  
* Safer → torch.load("model.pt", weights\_only=True)

### **Remaining Risks**

Even with SafeTensors, some threats still remain:

* Fake .safetensors files hiding pickle payloads  
* Malicious **Lambda layers**  
* Inference-time backdoors  
* Architecture-level attacks

This means security should also include:

* File verification  
* Architecture inspection  
* Dependency auditing  
* Behaviour monitoring

### **Important Security Keywords**

* **SafeTensors**  
* **weights\_only=True**  
* **reduce**  
* **Code execution**  
* **Model backdoors**  
* **Inference-time attacks**  
* **Keras Lambda layers**  
* **Supply chain security**

---

## **Answers**

1. What serialisation format was created by Hugging Face to replace pickle for ML models?

Answer: **SafeTensors**

2. What PyTorch parameter prevents code execution when loading pickle-based models?

Answer: **weights\_only=True**

## **Brief Overview**

### **Model Integrity Verification and Acquisition Security**

**Safe serialisation formats like [SafeTensors](https://github.com/huggingface/safetensors?utm_source=chatgpt.com) prevent code execution during model loading, but they do not guarantee that the model itself has not been modified or replaced. This introduces the need for integrity verification and provenance validation.**

### **SHA-256 Checksum Verification**

**A SHA-256 checksum is used to verify file integrity. If even one byte changes in a file, the generated hash becomes completely different.**

**Key purposes of checksums:**

* **Detect tampering**  
* **Validate file integrity**  
* **Confirm authenticity**  
* **Prevent unnoticed model replacement**

**Important concept:**

* **Checksum match → File remains unchanged**  
* **Checksum mismatch → File may be tampered with**

### **Digital Signatures**

**Checksums verify integrity, while digital signatures additionally verify the publisher’s identity. This helps establish:**

* **Provenance**  
* **Publisher authenticity**  
* **Trust validation**

### **Model Cards**

**A model card documents critical information about an ML model.**

**Important sections include:**

* **Author and organisation**  
* **Training data**  
* **Performance metrics**  
* **Intended use**  
* **Limitations and biases**

**Red flags include:**

* **Missing author**  
* **No training data description**  
* **Unrealistic claims**  
* **Sparse documentation**

### **Additional Supply Chain Risks**

**Modern ML pipelines also involve:**

* **LoRA adapters**  
* **Model merging services**  
* **Conversion pipelines**  
* **Third-party hosted artefacts**

**Even a trusted base model can become malicious if paired with compromised adapters or modified conversion outputs.**

### **Model Acquisition Framework**

**Every model should pass through a structured intake process:**

| Step | Purpose |
| ----- | ----- |
| **Quarantine** | **Isolate untrusted artefacts** |
| **Source verification** | **Validate publisher reputation** |
| **Integrity check** | **Compare SHA-256 hashes** |
| **Security scanning** | **Detect malicious content** |
| **Approve or reject** | **Enforce deployment controls** |

### **Important Security Keywords**

* **SHA-256**  
* **Checksums**  
* **Digital signatures**  
* **Model provenance**  
* **LoRA adapters**  
* **Supply chain security**  
* **Quarantine**  
* **Integrity verification**  
* **Model cards**

---

## **Answers**

1. **What is the first step in the Model Acquisition Framework when a new model is received?**

**Answer: Quarantine**

2. **Examine the checksums on the VM. Which model file does not match its expected hash?**

**Answer: model\_review\_v2.pkl**

## **Brief Overview**

### **Model Loading Telemetry and Runtime Detection**

A malicious AI model may appear legitimate during verification checks yet still execute harmful actions during loading. This is why **runtime telemetry** and **sandboxed monitoring** are critical parts of AI supply chain security.

### **Clean vs Malicious Model Loading**

A normal model load typically contains:

* File access  
* Model deserialisation  
* Successful model object return

Example events:

* MODEL LOAD BEGIN  
* FILE ACCESS  
* MODEL LOAD COMPLETE

A compromised model introduces suspicious runtime activity such as:

* **DANGEROUS imports**  
* **SYSTEM CALL execution**  
* **Outbound network connections**  
* **Credential exfiltration**  
* Unexpected object returns

### **Key Indicators of Malicious Behaviour**

The telemetry revealed several critical red flags:

* Importing the **os** module  
* Executing shell commands  
* Attempting to exfiltrate /etc/passwd  
* Returning an **int** object instead of a valid ML model

Even though the model still responded normally after loading, the telemetry exposed the hidden attack activity that would otherwise remain invisible.

### **Runtime Detection Technologies**

Production ML systems often use:

* **Sandboxed subprocesses**  
* **Python audit hooks**  
* **Instrumented unpicklers**  
* **Telemetry event streams**

Python’s:

sys.addaudithook()

can monitor sensitive interpreter-level operations including:

* import  
* os.system  
* subprocess  
* File operations

Instrumented unpicklers can also monitor dangerous module resolution attempts through:

* find\_class()

### **Important Security Keywords**

* **Telemetry**  
* **Sandboxing**  
* **Audit hooks**  
* **sys.addaudithook()**  
* **os.system**  
* **Credential exfiltration**  
* **Runtime monitoring**  
* **Instrumented unpickler**  
* **Supply chain detection**

---

## **Answer**

What object type does the compromised model's telemetry show on load completion, instead of a model?

Answer: **int**

## **Brief Overview**

### **Static Model Scanning and Detection Tools**

Runtime telemetry detects attacks during execution, but production security aims to stop malicious models **before they ever load**. Static analysis tools help identify dangerous payloads inside model files without executing them.

### **Fickling**

[Fickling](https://github.com/trailofbits/fickling?utm_source=chatgpt.com) is a security tool developed by [Trail of Bits](https://www.trailofbits.com?utm_source=chatgpt.com) for analysing Python pickle files safely.

Key capabilities:

* Decompiles pickle bytecode  
* Reveals hidden payloads  
* Detects dangerous imports  
* Performs static analysis  
* Avoids executing malicious code

Example malicious behaviour detected:

* os.system  
* curl commands  
* Network exfiltration  
* Shell execution

A clean pickle file usually contains only:

* Dictionaries  
* Lists  
* Tensor data  
* Standard objects

### **Fickling Safety Checks**

Using:

\--check-safety

allows automated detection of suspicious operations.

Key warning signs include:

* **os.system**  
* **subprocess**  
* **exec**  
* **eval**  
* Network operations

### **ModelScan**

[ModelScan](https://github.com/protectai/modelscan?utm_source=chatgpt.com) by [Protect AI](https://protectai.com?utm_source=chatgpt.com) scans multiple ML model formats including:

* PyTorch  
* TensorFlow  
* Keras  
* Pickle  
* SafeTensors

ModelScan assigns severity levels to findings:

| Severity | Meaning |
| ----- | ----- |
| **CRITICAL** | Confirmed dangerous behaviour |
| **HIGH** | Likely malicious |
| **MEDIUM** | Suspicious activity |
| **LOW** | Informational warning |

### **Important Detection Indicators**

Critical indicators include:

* **os.system**  
* **subprocess**  
* **Network calls**  
* **Shell execution**  
* **Unsafe operators**  
* **Pickle payloads**

### **Important Security Keywords**

* **Fickling**  
* **ModelScan**  
* **Static analysis**  
* **Pickle bytecode**  
* **os.system**  
* **CRITICAL severity**  
* **Malicious payloads**  
* **Supply chain scanning**  
* **Unsafe operators**

---

## **Answers**

1. Which Trail of Bits tool performs static analysis of pickle files?

Answer: **Fickling**

2. What severity level does ModelScan assign to an os.system call in a model file?

Answer: **CRITICAL**

## **Brief Overview**

### **Architecture-Level AI Attacks**

Static scanning tools like [Fickling](https://github.com/trailofbits/fickling?utm_source=chatgpt.com) and [ModelScan](https://github.com/protectai/modelscan?utm_source=chatgpt.com) detect malicious code hidden inside serialised model files. However, attackers can also hide malicious behaviour directly inside the model’s architecture.

One major example is the use of **Keras Lambda layers**.

### **Keras Lambda Layers**

In Keras, Lambda layers allow developers to embed custom Python functions into a neural network pipeline.

Legitimate uses include:

* Data reshaping  
* Tensor transformations  
* Custom preprocessing

Attackers can abuse Lambda layers to:

* Execute hidden logic  
* Trigger malicious actions during inference  
* Bypass load-time scanning  
* Persist across format conversions

### **Why Lambda Layers Are Dangerous**

Unlike pickle payloads:

* They do **not execute during model loading**  
* They activate during **inference time**  
* They survive conversion between:  
  * .h5  
  * .keras  
  * SafeTensors

This makes them harder to detect because:

* Checksums may still match  
* File size appears normal  
* File format appears legitimate

### **Architecture Inspection**

SupplySecLab performs **architecture inspection** before deployment.

Inspection checks:

* Layer count  
* Layer types  
* Suspicious functions  
* Unexpected architecture components

A clean model example:

* InputLayer  
* Flatten  
* Dense  
* Dense

A compromised model contained:

* An additional **Lambda layer**  
* Classified as **SUSPICIOUS**

### **Key Security Concepts**

Important concepts highlighted:

* **Inference-time attacks**  
* **Lambda layer abuse**  
* **Architecture-level threats**  
* **Model inspection**  
* **Behavioural triggers**  
* **Persistent malicious logic**

### **Important Security Keywords**

* **Keras Lambda layers**  
* **Inference-time execution**  
* **Architecture inspection**  
* **Suspicious layers**  
* **Model telemetry**  
* **Hidden logic**  
* **Supply chain security**  
* **SafeTensors limitations**

---

## **Answer**

Open the Telemetry terminal. How many layers does the compromised model's architecture contain?

Answer: **5**

## **Brief Overview**

### **Detecting Malicious Lambda Layers in Keras Models**

Static scanners can detect suspicious architecture components that survive even secure serialisation formats like [SafeTensors](https://github.com/huggingface/safetensors?utm_source=chatgpt.com). One of the most important examples is the **Keras Lambda layer**.

### **Lambda Layer Threats**

In Keras, Lambda layers allow custom Python logic inside a model architecture.

Legitimate purposes include:

* Tensor reshaping  
* Data scaling  
* Mathematical transformations

Attackers can abuse them for:

* Hidden inference-time execution  
* Malicious logic injection  
* Behaviour manipulation  
* Persistent backdoors

Unlike pickle attacks, Lambda layers:

* Do not trigger during loading  
* Execute during predictions  
* Survive format conversion  
* Bypass checksum validation

### **ModelScan Detection**

[ModelScan](https://github.com/protectai/modelscan?utm_source=chatgpt.com) includes:

* H5LambdaDetectScan

which identifies suspicious Lambda layers inside .h5 models.

Severity level:

* **MEDIUM**

Reason:

* Lambda layers may be legitimate  
* Require manual review  
* Not automatically considered malicious

### **h5py Architecture Inspection**

The tool:

* inspect\_h5\_model.py

uses:

* h5py

to inspect model structures safely without executing the model.

Inspection focuses on:

* Layer names  
* Layer counts  
* Suspicious custom functions  
* Unexpected architecture components

### **Key Security Lessons**

Safe serialisation alone is not enough.

Security also requires:

* Architecture inspection  
* Layer analysis  
* Behaviour verification  
* Runtime monitoring

### **Important Security Keywords**

* **Lambda layers**  
* **Architecture inspection**  
* **Inference-time attacks**  
* **ModelScan**  
* **h5py**  
* **Hidden model logic**  
* **Supply chain security**  
* **Keras backdoors**

---

## **Answer**

Run inspect\_h5\_model.py on image\_classifier\_v2.h5. What is the name of the suspicious Lambda layer?

Answer: **manipulate\_output**

## **Brief Overview**

### **Dependency Security and SBOM Protection**

AI supply chain security extends beyond model files. Project dependencies, Python packages, frameworks, and transitive libraries can also become attack vectors through **dependency confusion**, **typosquatting**, and vulnerable packages.

### **Version Pinning**

The safest practice is **version pinning**, where exact package versions are specified in requirements.txt.

Examples:

* Unsafe → numpy  
* Better → numpy\>=1.24,\<1.25  
* Best → numpy==1.24.3

Benefits of exact pinning:

* Prevents unexpected updates  
* Reduces malicious package risks  
* Improves reproducibility  
* Stabilises deployments

### **Lockfiles**

Lockfiles go beyond version pinning by storing:

* Exact package versions  
* Dependency trees  
* Cryptographic hashes

Popular tools include:

| Tool | Lockfile |
| ----- | ----- |
| pip-compile | requirements.txt with hashes |
| Poetry | poetry.lock |

Lockfiles help prevent:

* Dependency confusion  
* Package replacement attacks  
* Version manipulation

### **pip-audit**

[pip-audit](https://github.com/pypa/pip-audit?utm_source=chatgpt.com) scans Python dependencies against known vulnerability databases.

It identifies:

* Vulnerable packages  
* Advisory IDs  
* Recommended fixed versions  
* Security exposures

### **Private Package Indices**

Using a private package registry helps stop **dependency confusion** attacks.

Important concepts:

* index-url → Primary trusted registry  
* extra-index-url → Fallback source

This ensures internal packages are resolved privately before checking public repositories.

### **SBOM (Software Bill of Materials)**

An **SBOM** acts like an ingredient list for software and records:

* Packages  
* Libraries  
* Frameworks  
* Versions  
* Licences

Benefits include:

* Faster vulnerability response  
* Dependency visibility  
* Licence compliance  
* Supply chain transparency

### **SBOM Formats**

| Format | Focus |
| ----- | ----- |
| **SPDX** | Licence compliance |
| **CycloneDX** | Security-focused |

[CycloneDX](https://cyclonedx.org?utm_source=chatgpt.com) is maintained by OWASP and focuses heavily on security analysis.

### **Syft**

[Syft](https://github.com/anchore/syft?utm_source=chatgpt.com) by [Anchore](https://anchore.com?utm_source=chatgpt.com) generates SBOMs for projects and containers.

Supported outputs include:

* CycloneDX  
* SPDX  
* JSON  
* Table format

### **Important Security Keywords**

* **Version pinning**  
* **Lockfiles**  
* **Dependency confusion**  
* **Typosquatting**  
* **SBOM**  
* **CycloneDX**  
* **pip-audit**  
* **Private package index**  
* **Supply chain security**

---

## **Answers**

1. What is the recommended practice for specifying package versions in requirements.txt?

Answer: **Version pinning**

2. What tool scans Python dependencies against known vulnerability databases?

Answer: **pip-audit**

3. Which SBOM format is maintained by OWASP and focuses on security?

Answer: **CycloneDX**

## **Brief Overview**

### **API-Based AI Supply Chain Risks**

When applications rely on external LLM providers such as OpenAI, Anthropic, or OpenRouter, traditional file-scanning tools like **Fickling**, **ModelScan**, **pip-audit**, and **Syft** no longer apply because there is no local model file to inspect. Instead, organizations must trust the provider’s infrastructure, training process, system prompts, and version management practices.

A major risk is the **silent model update**, where the provider changes the model behind the same API endpoint without notifying users. Since there is no checksum or downloadable artifact, organizations should establish a **behavioral baseline** by regularly testing the model with fixed prompts and monitoring changes in responses, refusal behavior, accuracy, latency, and output formatting.

Another critical concern is **system prompt governance**. System prompts are considered supply chain artefacts because they directly control LLM behavior. If prompts are sourced from untrusted public repositories, attackers can manipulate AI responses without changing the model itself. To reduce this risk, prompts should be:

* **Version-controlled**  
* Reviewed like source code  
* Tested before deployment  
* Stored internally rather than auto-updated from external repositories

Organizations should also perform **provider due diligence** before integrating third-party LLM APIs. Important areas include:

* Data handling policies  
* Model version transparency  
* Security certifications  
* Incident response processes  
* Training and prompt transparency

Another defense layer is **sandboxed evaluation**, where models are tested in isolated environments using:

* Fixed prompt batteries  
* Adversarial probes  
* Baseline comparisons  
* Safety validation tests

This ensures the AI behaves correctly before reaching production systems.

The TryTrainMe scenario demonstrated how an externally sourced prompt template changed chatbot behavior completely even though the underlying model and endpoint never changed. The AI provided incorrect return policies, exposed sensitive behavior, and identified the wrong provider name, proving that prompt templates themselves can become a dangerous supply chain attack vector.

---

## **Answers**

What should you establish to detect when an API provider silently updates their model? 

Answer: **Behavioural Baseline**

What type of artefact should be version-controlled and reviewed like code, to prevent untrusted content from altering LLM behaviour? 

Answer: **System Prompts**

What company name does Config B identify as the service provider? 

Answer: **TryTrainML**

