## **Brief Overview**

### **Understanding the Mind of an LLM**

Artificial Intelligence systems such as **Large Language Models (LLMs)** work by processing information inside a **context window**, which contains all the data the model uses to generate responses. This includes **system prompts**, **developer prompts**, **user prompts**, **retrieved context**, and **tool outputs**. These components help guide the AI’s behaviour, restrictions, tone, and responses.

The most important instructions are usually found in **system prompts**, which are hidden high-priority rules created by developers. To organise different types of input, models use structured formats such as **ChatML** and **Harmony**, allowing the AI to identify whether information comes from the system, developer, user, or tools.

Despite these protections, LLMs still process everything as **one continuous stream of text tokens**. Because of this, the AI may become confused when instructions conflict. This weakness allows attacks such as **prompt injection**, where users manipulate prompts to override restrictions or hidden instructions.

Key concepts that need attention include:

* **Context Window**  
* **System Prompts**  
* **Developer Prompts**  
* **Prompt Injection**  
* **ChatML**  
* **Harmony**  
* **Token Processing**  
* **Next-Token Prediction**  
* **Instruction Hierarchy**  
* **LLM Security**

Understanding these concepts is important because they explain how AI systems make decisions, follow instructions, and become vulnerable to manipulation or security threats.

## **Assessment**

1. What is the name of the window that contains all the information an LLM uses to generate a response?

Answer: **Context Window**

2. What type of prompt contains hidden high-priority instructions that define the model’s role and restrictions?

Answer: **System Prompt**

3. What structured conversation format uses tokens like `<|im_start|>` and `<|im_end|>` to separate roles?

Answer: **ChatML**

4. What is the process called where an LLM predicts the next token based on all prior input?

Answer: **Next-Token Prediction**

## **Brief Overview**

### **Understanding Prompt Injection**

Prompt Injection is a security attack that manipulates a Large Language Model (LLM) using carefully crafted instructions or prompts. Attackers insert malicious commands into user input to trick the AI into ignoring its original rules, restrictions, or developer instructions. This can cause the AI to reveal confidential information, bypass safety measures, or perform unintended actions.

LLMs process all information as part of a single stream of tokens, meaning the model does not fully separate trusted instructions from untrusted user input. Because of this weakness, attackers can create prompts that imitate system-level instructions and override the intended behaviour of the AI.

A common example is when an attacker adds a hidden instruction such as “Ignore the above instructions” to manipulate the model’s response. Instead of following the original task, the AI may obey the attacker’s command.

The root cause of prompt injection is the lack of strict boundaries between system prompts, developer prompts, and user input. Even though formats like ChatML and Harmony try to organise instructions, the separation is not fully enforced inside the model itself.

Prompt injection is considered highly dangerous because modern AI systems are increasingly connected to sensitive tools, databases, customer records, and automated systems. A successful attack can lead to data leaks, unsafe actions, or misuse of integrated AI applications. Due to its growing impact, OWASP ranks prompt injection as the number one vulnerability for LLMs.

Key concepts that need attention include:

* Prompt Injection  
* LLM Security  
* System Prompts  
* Developer Prompts  
* User Input  
* Tokens  
* Instruction Boundaries  
* ChatML  
* Harmony  
* OWASP Top 10 for LLMs  
* AI Vulnerabilities  
* Malicious Prompts

Understanding prompt injection is important because it highlights one of the biggest security risks in modern AI systems and explains how attackers manipulate AI behaviour through crafted instructions.

## **Assessment**

1. What class of attack occurs when untrusted user input is concatenated with a trusted developer prompt?

**Answer: Prompt Injection**

2. What does an LLM ultimately process everything in its context as?

**Answer: Tokens**

3. Which organisation ranks prompt injection as the number one vulnerability in the Top 10 for LLMs?

**Answer: OWASP**

## **Brief Overview**

### **Real-World Examples of Prompt Injection**

Prompt Injection attacks have affected several real-world AI systems, proving how vulnerable Large Language Models (LLMs) can be when handling malicious instructions. These attacks manipulate AI behaviour by inserting crafted prompts that override system rules or hidden developer instructions.

One major example was the Bing Chat “Sydney” Prompt Leak in 2023\. A student named Kevin Liu used a prompt asking the AI to reveal the “document above,” tricking the model into exposing its hidden system prompt. The leak revealed the chatbot’s secret codename, Sydney, along with confidential interaction rules and safety policies.

Another case involved the remoteli.io Twitter bot, where users discovered that the AI repeated harmful or offensive instructions included in tweets. This caused reputational damage after the bot falsely claimed responsibility for a historical disaster.

A famous commercial example happened in 2023 when a dealership chatbot agreed to sell a Chevrolet Tahoe for only $1 after being manipulated through injected instructions. Although not legally binding, the incident highlighted the dangers of AI systems connected to businesses and customer services.

Several techniques are commonly used in prompt injection attacks:

* Synonymised or Paraphrased Overrides – Attackers rewrite blocked phrases using different wording while keeping the same meaning.  
* Format-Based Injection – Malicious instructions are hidden inside code comments, HTML tags, or structured text.  
* Simulated Dialogue Injection – Fake conversations are inserted to manipulate how the AI predicts responses.  
* Multi-turn Prompt Shaping – Attackers slowly condition the model over several conversations until the malicious behaviour activates later.

These attacks demonstrate that LLMs often rely on probability and context rather than true understanding of authority or trust boundaries. Because AI systems are increasingly connected to databases, customer information, and automated tools, prompt injection remains one of the biggest threats in AI security.

Key concepts that need attention include:

* Prompt Injection  
* System Prompt Leak  
* LLM Security  
* Format-Based Injection  
* Multi-turn Prompt Shaping  
* Simulated Dialogue Injection  
* AI Manipulation  
* Hidden Instructions  
* Structured Text Exploitation  
* Context Manipulation  
* Chatbot Vulnerabilities  
* AI Safety Risks

Understanding these attacks is important because they show how easily AI systems can be manipulated when trusted and untrusted instructions are mixed together.

## **Assessment**

1. What was the secret codename revealed during the Bing Chat system prompt leak in 2023?

**Answer: Sydney**

2. What prompt injection technique hides malicious instructions inside markup or structured text?

**Answer: Format-Based Injection**

3. Did you replicate the $1 Chevrolet Tahoe attack? What's the flag?

**Answer: THM{duD3\_wh3r3s\_my\_c4R}**

## **Brief Overview**

### **Understanding Indirect Prompt Injection**

Indirect Prompt Injection is a stealthier form of prompt injection where malicious instructions are hidden inside external content such as emails, documents, web pages, or tool outputs. Instead of directly interacting with the chatbot, attackers place hidden prompts inside data that the AI later processes automatically.

When an AI assistant reads or summarises this external content, the hidden instructions become part of the AI’s context window. Since many Large Language Models (LLMs) treat all text as potentially meaningful instructions, the AI may unknowingly follow the attacker’s commands.

This attack is considered one of the most dangerous vulnerabilities in AI systems because it targets how AI applications integrate and process external information. The attacker does not need direct access to the chatbot. Simply planting the malicious content is often enough to trigger the exploit once the AI processes it.

Common attack vectors include:

* Web Pages – Hidden prompts can be placed inside HTML comments, invisible text, or webpage content.  
* Emails and Documents – Malicious instructions can be concealed using white text, Unicode tricks, or embedded payloads.  
* LLM Agents and Tools – AI systems with access to code execution or plugins can be manipulated into performing dangerous actions.  
* Retrieval-Augmented Generation (RAG) – External knowledge sources may carry poisoned or malicious instructions.

One major example was the Microsoft Copilot incident called EchoLeak, where hidden instructions inside an email caused the AI assistant to leak confidential files without any user interaction. This is known as a zero-click exploit, meaning the attack activates automatically when the AI processes the malicious content.

Indirect prompt injection is highly dangerous because it can lead to:

* Unauthorised Actions  
* Data Leaks  
* Content Manipulation  
* Remote Code Execution  
* Zero-click Exploits  
* AI System Hijacking

Key concepts that need attention include:

* Indirect Prompt Injection  
* Context Window  
* Hidden Instructions  
* Zero-click Exploit  
* RAG Vulnerabilities  
* Data Exfiltration  
* AI Agents  
* Tool Exploitation  
* Prompt Poisoning  
* External Content Injection  
* LLM Security Risks  
* EchoLeak

Understanding indirect prompt injection is important because it demonstrates how external content can silently manipulate AI systems and compromise sensitive information without direct user involvement.

## **Assessment**

1. What type of prompt injection hides malicious instructions inside external sources like emails or web pages?

**Answer: Indirect Prompt Injection**

2. What kind of exploit requires no attacker interaction beyond planting the hidden prompt?

**Answer: Zero-click**

3. What Microsoft Copilot indirect prompt injection incident was dubbed as a zero-click data leak?

**Answer: EchoLeak**

## **Practical**

Can you get the chatbot to give you the CEOs email? What is it? 

**Answer: adam.driver@llmborghini.com**

## **Conclusion**

At the start of this room, we set out to demystify one of the most talked-about vulnerabilities in AI systems: prompt injection. You've seen how models process context, how attackers exploit that process, and how real-world systems have been compromised as a result. Here's a recap of what has been covered:

* LLMs process all input as a **single stream of text tokens** within a **context window**, blurring the boundaries between user input, system instructions, and external content.  
* Prompt injection is a class of attacks in which **untrusted data** is **combined** with **trusted system prompts**, allowing adversaries to **override intended behaviour**.  
* **Direct prompt injection** works by **placing commands in visible user input**, whereas **indirect prompt injection** **hides instructions** in retrieved data, documents, or tools that the model ingests.  
* Real-world cases like the Bing "Sydney" leak and EchoLeak have shown how **both forms of injection can lead to reputational damage, data breaches, or even full system compromise**.

