# **Understanding Software Supply Chains**

## **The Traditional Analogy**

# A software supply chain works similarly to building a house.

# When constructing a house, you do not create every component yourself. Instead, you rely on multiple suppliers:

* # Bricks from one supplier

* # Timber from another

* # Electrical wiring from another

* # Plumbing materials from another

# Each supplier also depends on their own suppliers for raw materials.

# This interconnected dependency network forms a **supply chain**.

# The security of the finished house depends on every component being trustworthy. Even if the walls are perfectly built, faulty wiring can still compromise the entire structure.

## **Software Supply Chains**

# Modern software development follows the same principle.

# A Python application rarely consists only of code written by the developer. Instead, it depends on external libraries and packages.

# For example:

# pip install torch

# Installing a package like torch automatically installs additional packages required for it to function.

# These additional packages are called dependencies.

# Some are installed directly by the developer, while others are installed automatically by existing dependencies.

## **Transitive Dependencies**

# A **transitive dependency** is a package pulled in automatically by another dependency rather than being explicitly installed by the developer.

# Example:

# You install: torch

# torch installs: filelock

# Even though you never requested filelock directly, your application still trusts and executes its code.

# This creates a long chain of trust where every dependency becomes part of the application’s attack surface.

## **Why Supply Chain Attacks Work**

# Supply chain attacks succeed because they exploit trust relationships instead of directly attacking the target application.

# Rather than compromising your system directly, attackers compromise software or components your system already trusts.

## **Real-World Examples**

| Incident | Year | What Happened | Impact |
| ----- | ----- | ----- | ----- |
| SolarWinds | 2020 | Attackers inserted malicious code into the Orion software build pipeline | \~18,000 organisations compromised |
| Log4Shell | 2021 | A vulnerability in the Log4j logging library enabled remote code execution | Millions of Java systems affected |

## **SolarWinds Attack**

# In the SolarWinds incident:

* # Attackers compromised the Orion build environment

* # Malicious code was inserted into legitimate software updates

* # Customers unknowingly installed the infected updates

# The malicious code was injected into the:

# build process

## **Why These Attacks Are Dangerous**

# Supply chain attacks scale extremely efficiently.

# Compromising one widely trusted dependency can affect:

* # Thousands of organisations

* # Millions of devices

* # Entire industries

# Victims often follow proper security practices yet still become compromised because the attack originates from a trusted upstream source.

## **Answer the Questions Below**

### **In the SolarWinds attack, where in the supply chain was the malicious code injected?**

# build process

### **While installing torch Pip also pulls in filelock, which you never listed. What type of dependency is filelock?**

# transitive dependency

# 

# **Understanding AI Supply Chains**

## **From Traditional Software to AI Systems**

Traditional software supply chains mainly involve:

* Source code  
* Libraries  
* Frameworks  
* Packages

AI systems introduce a completely new component:

* Trained machine learning models

Unlike ordinary software packages, AI models are not just code files. They are the result of an entire training pipeline.

## **What Makes Up a Trained Model?**

A trained AI model consists of several major elements:

* Model architecture  
* Training data  
* Training process  
* Serialised weights

These components together define how the model behaves.

## **Model Weights Analogy**

Model weights can be compared to a video game save file.

When saving a game, the save file stores:

* Character progress  
* Inventory  
* Position  
* Statistics  
* Completed missions

Similarly, model weights store everything the AI “learned” during training.

The danger is that downloading model files from untrusted sources is similar to downloading a save file from a stranger:

* The file may appear legitimate  
* The model may function correctly  
* Hidden malicious content may still exist inside

## **Why This Matters**

Many AI teams do not train models from scratch.

Instead, they use:

transfer learning

This means developers download an already-trained model and adapt it for their own tasks.

## **Fine-Tuning**

The most common transfer learning technique is:

fine-tuning

Fine-tuning adjusts a model’s weights using smaller task-specific datasets.

Advantages:

* Faster than training from scratch  
* Lower hardware requirements  
* Reduced costs

Security problem:

* Developers inherit trust dependencies from the original model creator

## **Inherited Backdoors**

Research has shown that malicious behaviour inserted during pre-training can survive fine-tuning.

This means:

* A poisoned base model can compromise downstream systems  
* Fine-tuning does not necessarily remove hidden backdoors  
* One malicious model can affect many applications

## **LoRA Adapters**

Modern systems often use:

LoRA adapters

These are lightweight files attached to existing models to modify behaviour.

This creates multiple trust layers:

* Base model trust  
* Adapter trust

Even if the base model is safe, a malicious adapter can still compromise the final system.

## **The Four Components of an AI Supply Chain**

An AI supply chain contains four major components.

### **1\. Datasets**

Training and evaluation data used during model development.

Examples:

* ImageNet  
* Common Crawl  
* Kaggle datasets

### **2\. Dependencies**

Supporting packages required by frameworks and applications.

Examples:

* NumPy  
* SciPy  
* Pillow

### **3\. Frameworks**

Libraries used to build and run machine learning systems.

Examples:

* PyTorch  
* TensorFlow  
* scikit-learn

### **4\. Models**

Pre-trained neural network architectures and weights.

Examples:

* BERT  
* GPT-2  
* Stable Diffusion

## **Why Trust Relationships Matter**

Every AI application depends on components created by other parties.

Typical trust chain:

Application → Framework → Registry → Package Maintainer

If any layer becomes compromised, the entire system may become compromised.

## **Model Loading Risks**

Many machine learning frameworks use:

pickle-based serialisation

Pickle files can contain executable Python objects.

This means that loading a malicious model file may execute code immediately during:

model.load()

This transforms model files into a potential remote code execution vector.

## **Answer the Questions Below**

### **What are the four key components of an AI supply chain? (listed alphabetically)**

datasets, dependencies, frameworks, models

### **What do model files contain that allows them to run code when loaded?**

serialised objects

# **Local LLM File Formats & AI Supply Chains**

## **What Is the Difference Between Downloading Models and Using APIs?**

Not every AI supply chain works the same way. There are two major ways organisations use AI models:

| Paradigm | Description |
| ----- | ----- |
| Downloading model files | You download model weights and run them locally on your own infrastructure |
| Calling models via API | You send prompts to a hosted provider that runs the model remotely |

Both approaches involve trust, but the risks are different.

---

# **Paradigm 1: Downloading Model Files**

This is the traditional machine learning approach.

You download files such as:

* .pkl  
* .pt  
* .bin  
* .safetensors  
* .gguf

from repositories like:

* [Hugging Face](https://huggingface.co?utm_source=chatgpt.com)  
* [PyTorch Hub](https://pytorch.org/hub/?utm_source=chatgpt.com)  
* [TensorFlow Hub](https://tfhub.dev?utm_source=chatgpt.com)

and run them on your own systems.

## **Why This Is Risky**

When you load a model file locally, it crosses your trust boundary.

Some formats can execute code automatically when loaded.

| File Format | Risk |
| ----- | ----- |
| .pkl, .pt, .bin | Use Python pickle serialization and may execute arbitrary code |
| .safetensors | Stores only raw weights and avoids executable serialization |
| .h5 | Keras format; not pickle-based but may contain executable architecture code |
| .gguf | Not pickle-based and safer from serialization attacks |

---

# **What Is GGUF?**

GGUF is the dominant format for running local large language models such as:

* Meta Platforms LLaMA  
* Mistral AI Mistral  
* Alibaba Cloud Qwen

It is commonly used with tools like:

* llama.cpp

## **Why GGUF Became Popular**

GGUF models are usually quantized.

### **Quantization**

Quantization reduces model precision:

* From 32-bit floating point  
* To 4-bit or 8-bit integers

This makes models:

* Smaller  
* Faster  
* Easier to run on consumer hardware

However, if a third party performed the quantization, you must trust every transformation applied to the model before you downloaded it.

---

# **Paradigm 2: Calling Models Through APIs**

Instead of downloading weights, you call hosted services such as:

* [OpenAI](https://openai.com?utm_source=chatgpt.com)  
* [Anthropic](https://www.anthropic.com?utm_source=chatgpt.com)  
* [Google AI](https://ai.google?utm_source=chatgpt.com)  
* [OpenRouter](https://openrouter.ai?utm_source=chatgpt.com)

You send prompts and receive responses.

You never see the actual model files.

---

# **Why APIs Still Have Supply Chain Risks**

API access removes pickle-file risks, but you still trust the provider’s pipeline.

| Supply Chain Element | Download Paradigm | API Paradigm |
| ----- | ----- | ----- |
| Model weights | You can inspect them | Provider controls them |
| Training data | Often documented | Usually hidden |
| Fine-tuning | You control it | Provider may change it |
| Versioning | You pin hashes | Provider may silently update |
| Hosting | Your infrastructure | Shared provider infrastructure |

## **Key Insight**

Using an API does not remove the AI supply chain.

It simply hides the supply chain behind a remote service.

You are trusting:

* The provider’s training process  
* Their security  
* Their infrastructure  
* Their update pipeline  
* Their hidden system prompts

---

# **Answer**

## **What is the dominant file format for running local large language models such as LLaMA, Mistral, and Qwen?**

**GGUF**

# **AI Supply Chain Attack Vectors**

## **The Four Attack Layers**

AI supply chains contain multiple trust boundaries, and attackers only need to compromise one layer to impact downstream systems.

The four major attack layers are:

| Layer | Description |
| ----- | ----- |
| Model Layer | Attacks embedded directly inside model files or weights |
| Dependency Layer | Malicious packages and libraries |
| Data Layer | Poisoned datasets used during training |
| Infrastructure Layer | Compromised repositories, CI/CD pipelines, or maintainer accounts |

---

# **Layer 1: The Model Layer**

The model layer is the most unique part of AI supply chain security.

Attackers can:

* Embed executable code inside model files  
* Modify model architectures  
* Alter trained weights to create hidden behaviours

## **Three Types of Model Attacks**

| Attack Type | Description |
| ----- | ----- |
| Serialisation-level | Malicious code executes when the file is loaded |
| Architecture-level | Hidden logic embedded inside neural network layers |
| Weights-level | Trained weights altered to trigger malicious behaviour |

---

# **Serialisation-Level Attacks**

These attacks exploit dangerous file formats such as:

* .pkl  
* .pt  
* .bin

which use Python pickle serialization.

The payload executes automatically when the model is loaded.

## **SafeTensors Protection**

.safetensors eliminates:

* **Serialisation-level attacks**

because it stores only raw tensor weights and no executable objects.

However, SafeTensors does NOT prevent:

* Architecture-level attacks  
* Weights-level backdoors

---

# **Layer 2: The Dependency Layer**

This layer targets ML libraries and packages.

Common attacks include:

| Attack | Description |
| ----- | ----- |
| Dependency confusion | Malicious package replaces internal dependency |
| Typosquatting | Fake package mimics a legitimate name |
| Malicious packages | Malware hidden inside packages |

Example:

Installing:

pip install torch

may also automatically install:

* filelock  
* typing-extensions  
* sympy

These are called:

* **transitive dependencies**

because they were not directly requested.

---

# **Layer 3: The Data Layer**

Machine learning models depend entirely on training data.

Attackers can poison datasets by inserting carefully crafted samples.

Research shows that replacing as little as:

* **0.1%**

of a dataset can create reliable backdoors.

Some denial-of-service poisoning attacks require only:

* **0.001%**

modification.

## **Key Insight**

Poisoned datasets may still produce models with:

* Normal accuracy  
* Normal benchmarks  
* Hidden triggers

making detection extremely difficult.

---

# **Layer 4: The Infrastructure Layer**

This layer targets the systems hosting AI artifacts.

Attackers may:

* Compromise model repositories  
* Hijack CI/CD pipelines  
* Steal maintainer credentials  
* Push malicious updates under trusted identities

Examples include:

* Compromised [Hugging Face](https://huggingface.co?utm_source=chatgpt.com) accounts  
* Malicious uploads to [PyPI](https://pypi.org?utm_source=chatgpt.com)  
* Poisoned GitHub Actions pipelines via [GitHub](https://github.com?utm_source=chatgpt.com)

---

# **The Compounding Effect**

Sophisticated attacks combine multiple layers simultaneously.

Example attack chain:

1. Compromise a model repository account  
2. Upload a malicious pickle model  
3. Include typosquatted dependencies  
4. Distribute poisoned datasets

One download can therefore compromise:

* The model  
* The dependencies  
* The training data  
* The infrastructure pipeline

all at once.

---

# **Answers**

## **At which layer of the AI supply chain do pickle-based attacks occur?**

**Model Layer**

## **Which level of model attack is eliminated by converting to SafeTensors format?**

**Serialisation-level**

## **Researchers find that 0.1% of a public training dataset has been replaced with crafted samples designed to introduce a backdoor. Which attack layer does this represent?**

**Data Layer**

# **Real AI Supply Chain Incidents (2022–2025)**

## **Incident 1: PyTorch Dependency Confusion (December 2022\)**

A malicious package named:

* torchtriton

was uploaded to [PyPI](https://pypi.org?utm_source=chatgpt.com).

It exploited:

* pip version resolution  
* dependency confusion

because internal PyTorch nightly builds referenced a package with the same name.

The malicious package stole:

* SSH keys  
* Git configs  
* /etc/passwd  
* files from user home directories

and exfiltrated data through encrypted DNS queries.

## **Key Lesson**

Even trusted package managers can install attacker-controlled packages if dependency resolution is misconfigured.

### **Attack Layer**

* **Dependency Layer**

---

# **Incident 2: Hugging Face Hub Vulnerabilities (2023–2024)**

Researchers discovered:

* Over 1,500 exposed API tokens  
* Hundreds with write permissions

on [Hugging Face](https://huggingface.co?utm_source=chatgpt.com).

This allowed attackers to:

* Push malicious updates  
* Modify trusted repositories

Researchers also identified:

* malicious pickle-based models

that appeared legitimate while executing hidden code during loading.

## **Key Lesson**

Trusted model repositories can distribute malicious artifacts even when models appear functional and professional.

### **Attack Layers**

* Infrastructure Layer  
* Model Layer

---

# **Incident 3: Ultralytics Build Pipeline Compromise (December 2024\)**

Attackers compromised:

* GitHub Actions build workflows

for Ultralytics.

The attack embedded:

* a cryptominer

into published PyPI packages.

Developers performing normal:

pip install

operations unknowingly installed malware.

## **Key Lesson**

CI/CD pipelines are critical trust boundaries in modern software supply chains.

### **Attack Layer**

* **Infrastructure Layer**

---

# **Incident 4: @solana/web3.js Compromise (December 2024\)**

Attackers stole:

* npm maintainer credentials

and published malicious versions of:

* @solana/web3.js

The backdoor exfiltrated:

* cryptocurrency private keys

from wallet users.

Estimated losses reached:

* $130,000–$184,000

before detection.

## **Key Lesson**

Trusted maintainer identities are high-value targets because users inherently trust official package updates.

### **Attack Layer**

* **Infrastructure Layer**

---

# **Incident 5: NullifAI Scanner Evasion (February 2025\)**

Researchers at ReversingLabs discovered that:

* deliberately corrupted pickle files

could bypass automated security scanners on [Hugging Face](https://huggingface.co?utm_source=chatgpt.com).

The models:

* passed platform scans  
* still executed malicious code locally

when loaded.

## **Key Lesson**

Automated scanning alone cannot guarantee model safety.

### **Attack Layer**

* **Model Layer**

---

# **Answers**

## **The torchtriton package exploited pip's version resolution to install a public package over an internal one. Which of the four attack layers does this target?**

**Dependency Layer**

## **The @solana/web3.js attacker stole a maintainer's credentials to push malicious updates to a legitimate, high-trust repository. Which attack layer does this represent?**

**Infrastructure Layer**

# **Evaluating a Hugging Face Model Repository**

## **What to Check Before Deploying a Model**

Before running:

pip install

or:

model.load()

you should evaluate several trust signals.

---

# **Signals to Review**

| Signal | What to Look For |
| ----- | ----- |
| Organisation | Verified account and reputation |
| Downloads | Real usage and adoption |
| File Format | Prefer SafeTensors over pickle formats |
| Security Scan | Scanner warnings or malicious findings |
| Model Card | Detailed documentation and limitations |
| Community | Discussions, warnings, or suspicious behaviour |

---

# **Suspicious Model Indicators**

The suspicious model repository showed multiple warning signs:

| Indicator | Risk |
| ----- | ----- |
| Unverified organisation | Unknown trustworthiness |
| Very low download count | Little community validation |
| Pickle-based files | Possible code execution |
| Sparse model card | Lack of transparency |
| Security warnings | Potential malicious behavior |

---

# **Verified Model Baseline**

The comparison model:

* google-bert/bert-base-uncased

from Google demonstrated good supply chain hygiene.

## **Positive Signals**

| Signal | Verified Model |
| ----- | ----- |
| Verified organisation | Yes |
| Established track record | 6+ years |
| Millions of downloads | Yes |
| File format | SafeTensors |
| Detailed documentation | Yes |
| Security scan issues | None |

---

# **Why SafeTensors Matters**

SafeTensors avoids:

* pickle deserialization  
* arbitrary code execution during loading

Unlike:

* .pkl  
* .pt  
* .bin

SafeTensors stores only:

* raw tensor weight values

making it significantly safer.

# **Answers**

## **In the static site, what is the name of the unverified organisation that uploaded the model?**

**trustworthy-ai-models**

## **How many downloads does this model have (last month)?**

**127**

## **What file format does the verified model (google-bert/bert-base-uncased) use for its weights?**

**SafeTensors**

**Conclusion**

The attacks in this room share a common thread. The torchtriton package stole keys from developers who ran a routine installation command. The malicious models on Hugging Face performed their advertised ML task correctly while silently connecting to servers controlled by attackers. The Solana package compromise lasted only hours but caused six-figure losses. **None of these attacks required breaking into anything**. In each case, the victim trusted a component that had earned that trust, and that trust was the vulnerability.

That is what makes supply chain attacks particularly hard to defend against. The malicious models JFrog found on Hugging Face were not obviously wrong. They had professional model cards, plausible organisation names, and real functionality (they performed their advertised task correctly). They passed every informal check an ML engineer would reasonably run. The compromise was invisible until anomalous network connections surfaced, sometimes weeks later.

**Key takeaways from this room:**

* **Model files are code in disguise.** Pickle-based models execute at load time; format choice determines your exposure.  
* **The four attack layers are distinct.** Model, dependency, data, and infrastructure attacks require different defences and produce different forensic signals.  
* **Transitive dependencies silently expand your attack surface.** You are responsible for every package pip installs, not just the ones you listed.  
* **Automated scanners are not a complete defence.** The NullifAI case shows that malformed files can pass platform checks while remaining executable.  
* **Infrastructure compromise beats content filtering.** Stolen credentials on a trusted repository push malicious code with full legitimacy; no tooling flags it at download time.  
* **SafeTensors eliminates serialisation-level attacks.** It does not eliminate architecture-level or weight-level attacks; the format is a single control, not a complete solution.

