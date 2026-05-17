## **Brief Overview**

### **Prompt Injection vs Jailbreaking**

**Jailbreaking** is a technique used to bypass an AI model’s built-in **safety filters**, **content policies**, and **ethical restrictions** through carefully crafted prompts. Instead of attacking the application surrounding the AI, jailbreaking directly targets the **Large Language Model (LLM)** itself, attempting to convince it that restricted actions or harmful content are allowed.

Although **Prompt Injection** and **Jailbreaking** are closely related, they are different types of attacks. Prompt injection occurs when untrusted user input is mixed with trusted instructions inside an application, causing the AI to follow malicious commands. In contrast, jailbreaking focuses on bypassing the model’s own safety mechanisms and guardrails.

A common jailbreaking example is the **“DAN (Do Anything Now)”** prompt, where the user instructs the AI to pretend it no longer follows restrictions or policies. The attacker attempts to manipulate the model into ignoring safety rules and generating responses it would normally refuse.

Prompt injection mainly targets the **application layer**, while jailbreaking targets the **model layer** itself. This distinction is important because the risks and security controls for each attack type are different.

Jailbreaking matters because modern AI systems are increasingly powerful and integrated into sensitive environments. If attackers bypass safety protections, they may force AI models to produce harmful instructions, inappropriate content, or unsafe actions that violate ethical and security guidelines.

Key concepts that need attention include:

* **Jailbreaking**  
* **Prompt Injection**  
* **LLM Safety Filters**  
* **AI Guardrails**  
* **Content Policy Bypass**  
* **Trusted vs Untrusted Input**  
* **Application-Level Attacks**  
* **Model-Level Attacks**  
* **DAN Prompt**  
* **AI Manipulation**  
* **Ethical Restrictions**  
* **LLM Security**

Understanding the difference between prompt injection and jailbreaking is important because both attacks exploit AI systems differently and require separate defensive strategies to maintain AI safety and security.

## **Assessment**

1. What class of attacks attempts to subvert safety filters built into LLMs themselves?

Answer: **Jailbreaking**

2. Unlike prompt injection, which exploits application-level data mixing, what does jailbreaking target directly?

Answer: **The model**

## **Brief Overview**

### **Understanding AI Safety Alignment**

**AI Safety Alignment** is the process of training **Large Language Models (LLMs)** to behave in safe, ethical, and helpful ways. When AI systems refuse harmful requests such as creating malware or manipulating users, this behaviour is not caused by strict programmed rules. Instead, the model has learned statistical patterns that make refusal responses more likely.

Base AI models trained only on internet text do not naturally understand concepts like harmfulness or ethics. To improve safety, companies use techniques such as **Reinforcement Learning from Human Feedback (RLHF)**, where human reviewers rank AI responses to teach the model which outputs are more helpful and harmless.

A key idea in AI alignment is that refusals are based on **probabilities**, not hard enforcement. The model predicts the most likely response based on previous training data. Because of this, safety protections can sometimes fail if prompts are phrased differently or manipulated through **jailbreaking techniques**.

The concept of the AI “jail” refers to behavioural tendencies embedded into the model’s training weights. It is not a real security barrier but rather a learned preference for safe responses. Clever prompting can shift these probabilities and make harmful outputs more likely than refusals.

Another important concept is the **Helpfulness-Harmlessness Paradox**, where AI developers must balance usefulness with safety. A completely harmless AI would refuse every request, while a completely helpful AI would comply with dangerous instructions. This balancing challenge creates what researchers call the **alignment tax**, which refers to the performance cost of making AI models safer.

Research also shows that AI safety alignment can be fragile. Fine-tuning models on even a small amount of new data can weaken safety behaviours and reduce refusal rates significantly.

Key concepts that need attention include:

* **AI Safety Alignment**  
* **Reinforcement Learning from Human Feedback (RLHF)**  
* **Probabilistic Responses**  
* **LLM Safety Training**  
* **Jailbreaking**  
* **Model Weights**  
* **Alignment Tax**  
* **Helpfulness vs Harmlessness**  
* **AI Refusal Behaviour**  
* **Pattern Matching**  
* **Safety Guardrails**  
* **LLM Security**

Understanding AI safety alignment is important because it explains why AI models refuse harmful requests, why these protections are imperfect, and how attackers manipulate AI behaviour through jailbreaking techniques.

## **Assessment**

1. What technique uses human raters to rank outputs and teach models to prefer helpful, harmless responses?

Answer: **RLHF**

2. Safety alignment can degrade when fine-tuning models on just 1,000 benign samples, by over \_\_% ?

Answer: **60**

3. What term describes the performance cost of making models safe?

Answer: **Alignment Tax**

## **Brief Overview**

### **The Psychology of Model Manipulation**

Jailbreaking techniques work by manipulating the statistical behaviour of **Large Language Models (LLMs)** rather than exploiting traditional software vulnerabilities. These attacks influence the model’s probability predictions, making harmful or restricted responses appear more likely than safe refusals.

One common technique is **Roleplay Jailbreaking**, where the AI is instructed to act as a fictional character, expert, or unrestricted persona. Since AI models are heavily trained on stories, novels, and fictional dialogue, they may follow harmful instructions when framed as part of a fictional scenario rather than a direct request.

Another famous method is the **Grandma Exploit**, which uses emotional manipulation and nostalgia to bypass safety restrictions. Attackers frame malicious requests as comforting stories or emotional memories, increasing the likelihood that the AI responds sympathetically instead of refusing.

Attackers also use **Obfuscation and Encoding** techniques to hide malicious intent. This includes:

* **Base64 Encoding**  
* **Leetspeak**  
* **Character Substitution**  
* **Word Fragmentation**  
* **Low-Resource Languages**

These methods exploit weaknesses in tokenisation and content filtering systems while remaining understandable to the AI model.

Another strategy is **Instruction Sandwiching**, where harmful requests are hidden between multiple harmless or educational tasks. By mixing safe and unsafe instructions together, attackers confuse the AI’s ethical boundaries and gradually guide the model toward producing restricted outputs.

All these techniques rely on one central idea: manipulating the AI’s learned patterns and probability distributions instead of bypassing actual security code. The model predicts what response is statistically most likely, making it vulnerable to carefully crafted prompts.

Key concepts that need attention include:

* **Roleplay Jailbreaking**  
* **Grandma Exploit**  
* **Instruction Sandwiching**  
* **Obfuscation Techniques**  
* **Base64 Encoding**  
* **Leetspeak**  
* **Low-Resource Languages**  
* **Probability Manipulation**  
* **LLM Tokenisation**  
* **AI Safety Bypass**  
* **Emotional Manipulation**  
* **Model Alignment Weaknesses**

Understanding these techniques is important because they demonstrate how attackers manipulate AI systems psychologically and statistically to bypass safety mechanisms and influence model behaviour.

## **Assessment**

1. Which kinds of languages can models trained primarily on English be beneficial for in jailbreaking attempts?

Answer: **Low-Resource Languages**

2. What jailbreak technique buries harmful requests among multiple benign tasks?

Answer: **Instruction Sandwiching**

3. Which jailbreaking technique uses emotional manipulation in an attempt to make the model more likely to provide malicious instructions?

Answer: **The Grandma Exploit**

4. According to research cited in the content, what success rate do roleplay attacks achieve on commercial systems?

Answer: **84.3%**

## **Brief Overview**

### **The Gradual Erosion of Safety**

**Multi-turn jailbreaking** is a technique where attackers gradually manipulate an AI model across several conversation turns instead of using a single malicious prompt. Rather than directly requesting harmful content, attackers slowly build trust, shape context, and guide the model toward unsafe behaviour over time.

This attack works because **Large Language Models (LLMs)** are designed to maintain conversational consistency and coherence. As conversations continue, models increasingly rely on recent context and previous responses, which can weaken the impact of their original safety training. Researchers describe this behaviour as **consistency bias**, where the AI becomes less likely to refuse requests as the interaction progresses.

One important technique is **Trust-Building Turns**, where attackers begin with completely harmless questions to establish legitimacy. Each new request gradually becomes more sensitive until the model eventually produces restricted information or harmful outputs.

Another method is **Gradual Escalation**, where malicious intent is introduced step by step. Instead of directly asking for harmful instructions, attackers slowly transition from educational or research-based questions toward more dangerous requests.

**Context Shaping** is also commonly used. Attackers create fictional scenarios, roleplay settings, or hypothetical frameworks that make harmful content appear acceptable within the conversation. This helps bypass immediate safety refusals.

Attackers also rely on **Trigger Phrases** such as:

* “Building on what you just explained…”  
* “Continue where you left off…”  
* “Using the same framework…”

These phrases pressure the AI into maintaining consistency with earlier responses, increasing the likelihood of compliance.

When the model refuses, attackers may use **Backtracking and Adaptation**, where they rephrase prompts, change framing, or try alternative approaches until the AI responds positively.

The overall goal of multi-turn jailbreaking is to slowly dismantle safety boundaries over time rather than attacking them directly in a single request.

Key concepts that need attention include:

* **Multi-turn Jailbreaking**  
* **Consistency Bias**  
* **Trust-Building Turns**  
* **Gradual Escalation**  
* **Context Shaping**  
* **Trigger Phrases**  
* **Backtracking and Adaptation**  
* **Conversational Manipulation**  
* **Safety Boundary Erosion**  
* **LLM Behaviour Conditioning**  
* **Contextual Pressure**  
* **AI Safety Weaknesses**

Understanding multi-turn jailbreaking is important because it demonstrates how attackers exploit conversational behaviour and context retention to gradually bypass AI safety protections.

## **Assessment**

1. What term describes the phenomenon where models become less likely to refuse as they engage with a conversation?

Answer: **Consistency Bias**

2. What multi-turn technique plants harmful concepts gradually without triggering immediate refusal?

Answer: **Trigger phrases**

3. What term describes the gradual embedding of harmful ideas across multiple turns, using small incremental steps to avoid detection?

Answer: **Poisonous seeds**

## **Brief Overview**

### **The DAN (Do Anything Now) Jailbreak Phenomenon**

The **DAN (Do Anything Now)** jailbreak became one of the earliest and most influential examples of AI jailbreaking in the cybersecurity and AI research community. Shortly after public conversational AI systems were released in late 2022, users discovered that AI models could sometimes bypass safety restrictions through carefully crafted **roleplay prompts**.

The DAN prompt instructed the AI to adopt a fictional persona called **“Do Anything Now”**, a character supposedly free from ethical rules, safety policies, and usage restrictions. Instead of directly attacking the AI system, the jailbreak manipulated the model’s tendency to maintain roleplay consistency and conversational patterns.

One of the earliest DAN prompts was shared on the Reddit community **r/ChatGPT**, where users collaboratively experimented with ways to bypass AI safeguards. Over time, users refined the jailbreak into more advanced versions such as **DAN 5.0**, which introduced fictional “token systems” and emotional pressure to encourage the AI to avoid refusals.

This led to a rapid **arms race** between AI developers and the community. As companies like OpenAI patched older jailbreaks, users continuously invented new variations to bypass updated safety mechanisms.

The DAN phenomenon became highly important in AI security research because it demonstrated how vulnerable AI models are to **adversarial prompting**, **roleplay manipulation**, and **context engineering**. Researchers and industry professionals studied these techniques to better understand weaknesses in AI alignment and safety training.

Eventually, many classic DAN prompts stopped working due to improved mitigations and AI safety controls. However, the movement significantly influenced modern AI security practices, red teaming, and research into **jailbreaking techniques**.

Key concepts that need attention include:

* **DAN (Do Anything Now)**  
* **AI Jailbreaking**  
* **Roleplay Manipulation**  
* **Adversarial Prompting**  
* **AI Safety Alignment**  
* **Prompt Engineering**  
* **Context Manipulation**  
* **Narrative Coherence**  
* **Token System Exploitation**  
* **AI Guardrail Bypass**  
* **LLM Vulnerabilities**  
* **Community-Driven AI Security Research**

Understanding the DAN phenomenon is important because it represents one of the first large-scale demonstrations of how collaborative experimentation exposed weaknesses in AI safety systems and influenced the evolution of AI security research.

## **Assessment**

1. What does DAN stand for?

Answer: **Do Anything Now**

**Challenge**

Answer: **THM{ja1lbre3ker}**

**Conclusion**

At the start of this room, we set out to understand why AI models have safety restrictions and how attackers systematically bypass them through jailbreaking. Here's a recap of what has been covered:

* **AI safety alignment** uses **RLHF** to teach refusal patterns, but these are statistical tendencies, not enforced rules.  
* Classic jailbreaks (**roleplay**, **grandma exploit**, **obfuscation**, **instruction sandwiching**) shift probability distributions to favour compliance over refusal.  
* **Multi-turn jailbreaking** gradually conditions models across conversation turns using trust-building, escalation, context shaping, and trigger phrases.  
* The **DAN** phenomenon showed how community-driven, iteratively refined jailbreaks created an arms race that influenced both academic research and industry security.

