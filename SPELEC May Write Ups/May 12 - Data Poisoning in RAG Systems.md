## **Brief Overview**

Training Data Poisoning is a security attack where attackers manipulate the data used to train or fine-tune an AI model. Instead of attacking the model directly, they influence what the model learns from. This causes the AI to produce incorrect, biased, or harmful outputs while still appearing to function normally.

Modern AI systems commonly depend on:

* **External Data Sources**  
* **Knowledge Bases**  
* **Automated Ingestion Pipelines**  
* **Embeddings**  
* **Fine-Tuning Datasets**  
* **Retrieval Systems**

These systems often trust ingested data automatically. Attackers exploit this by inserting malicious or misleading content into the training or retrieval pipeline.

A major concept is **Inference-Time Influence**, where poisoned information continues affecting outputs long after the original malicious source is removed. The model remembers statistical patterns rather than exact files.

Poisoning attacks are dangerous because:

* No direct system exploit is required  
* No prompt injection is needed  
* No modification of model code happens  
* The AI behaves exactly as trained

Attackers only need to influence what the system reads, stores, or learns from.

Common poisoning methods include:

* **Training Data Poisoning**  
* **Fine-Tuning Poisoning**  
* **Knowledge Base Poisoning**  
* **Embedding Manipulation**  
* **Context Manipulation**  
* **Data Drift**

During training, models use **Gradient Descent** to update internal weights based on prediction errors. Repeated poisoned examples gradually shift the model’s behaviour over time.

Possible effects include:

* Incorrect security guidance  
* Biased recommendations  
* Misinformation  
* Manipulated outputs  
* Persistent behavioural changes

The **Microsoft Tay (2016)** incident is a well-known example. Attackers fed offensive content into the chatbot’s learning system, causing harmful responses. The model itself was not hacked; the learning data was poisoned.

Important security concerns include:

* **Persistent Behaviour Changes**  
* **Long-Term Integrity Damage**  
* **Hidden Manipulation**  
* **Trust Boundary Abuse**  
* **Automated Learning Exploitation**  
* **Difficult Detection**

Poisoned AI outputs often appear:

* Logical  
* Confident  
* Well-written  
* Authoritative

This makes poisoning attacks difficult to identify in real-world systems.

Important keywords and concepts:

* **Training Data Poisoning**  
* **Fine-Tuning**  
* **Gradient Descent**  
* **Embeddings**  
* **Inference-Time Influence**  
* **Knowledge Base Poisoning**  
* **Model Integrity**  
* **Automated Ingestion**  
* **Persistent Attacks**  
* **AI Supply Chain Security**  
* **Semantic Manipulation**  
* **Data Drift**  
* **Context Injection**  
* **Trust Boundaries**

---

What type of poisoning affects model training?

Answer: Training data poisoning

## **Brief Overview**

Modern AI systems use **Embeddings** to represent text as numerical vectors based on meaning rather than exact wording. These embeddings are stored inside a **Vector Database**, where documents are retrieved using **Semantic Similarity**. When users submit a query, the system converts it into an embedding and retrieves the closest matching documents before passing them to the LLM as context.

This process introduces major security risks because retrieval is based on **relevance**, not truth, authority, or safety. If attackers manipulate the embedding space, poisoned documents can outrank legitimate ones and influence model outputs.

One major attack is **Embedding-Level Poisoning**, where attackers manipulate how documents are represented in vector space without modifying the base model itself. Instead of changing model weights, the attacker changes retrieval behaviour.

Another important attack is **Corpus Poisoning**, where attackers flood the vector database with malicious or misleading documents to dominate retrieval rankings.

Common corpus poisoning techniques include:

* **Keyword Stuffing** — repeating common search phrases to increase retrieval probability  
* **Semantic Mimicry** — imitating the tone and structure of trusted documents  
* **Duplication** — uploading many slightly modified copies of the same malicious content  
* **Corpus Flooding** — increasing the density of attacker-controlled documents within a semantic region

The retrieval system uses **Top-k Similarity Search**, meaning only the highest-ranked documents are shown to the model. Legitimate documents may still exist in the system but become ineffective because poisoned documents rank higher.

A key mathematical concept used is **Cosine Similarity**, which measures how close vectors are in semantic space. Small changes in phrasing can move malicious documents closer to common queries.

Important distinctions between poisoning attacks:

* **Training Data Poisoning** → changes model weights during training  
* **Embedding-Level Poisoning** → manipulates semantic representation in vector space  
* **Corpus Flooding** → increases retrieval probability using many related poisoned documents

A real-world example occurred in **2023**, when researchers from **Prompt Security** demonstrated a poisoning attack against a RAG system using:

* **LangChain**  
* **Chroma**  
* **Sentence-Transformers**  
* **Llama 2**

A single malicious document hidden inside the vector database appeared in nearly **80%** of tested retrievals because it was semantically similar to common topics. The model and prompts were never modified; retrieval rankings alone changed the output behaviour.

Key security risks include:

* **Inference-Time Manipulation**  
* **Hidden Retrieval Abuse**  
* **Semantic Ranking Manipulation**  
* **Context Injection**  
* **Knowledge Base Poisoning**  
* **Top-k Retrieval Bias**  
* **Trust Boundary Abuse**  
* **Dynamic Retrieval Exploitation**

Important keywords and concepts:

* **Embeddings**  
* **Vector Database**  
* **Cosine Similarity**  
* **Semantic Similarity**  
* **Corpus Poisoning**  
* **Embedding-Level Poisoning**  
* **Corpus Flooding**  
* **Semantic Mimicry**  
* **Keyword Stuffing**  
* **Top-k Retrieval**  
* **Inference-Time Attacks**  
* **Retrieval Manipulation**  
* **RAG Security**  
* **Vector Space Manipulation**  
* **Nearest-Neighbour Search**

---

Which corpus poisoning technique increases the density of attacker-controlled documents in a specific semantic region?

Answer: Corpus flooding

Which type of poisoning manipulates how documents are represented in vector space without changing the base model?

Answer: Embedding-level poisoning

What corpus poisoning technique imitates the tone and structure of trusted documents?

Answer: Semantic mimicry

**Conclusion**

Data and model poisoning change how trust operates in AI systems by targeting what the model learns and what it retrieves. Instead of attacking prompts or application code, poisoning manipulates the data layer. When that layer is compromised, the model behaves differently while still appearing to function normally. In this room, you learned that poisoning can occur at multiple stages: training data, embedding and corpus storage, and ingestion pipelines. These attacks do not require direct access to model weights or user prompts. By influencing trusted data sources, attackers can create subtle behavioural drift or obvious manipulation at scale. Poisoning is dangerous because it targets assumptions about data integrity. The model continues to operate as designed. The failure lies in what it was allowed to learn or retrieve.

## Key Takeaways

* Control over data can equal control over behaviour  
* Embedding and ranking manipulation can influence outputs without retraining  
* Automation amplifies poisoning risk at scale  
* Subtle behavioural drift is often more dangerous than obvious failure  
* No single detection mechanism is sufficient

## Framework Alignment

The risks explored in this room align with how modern AI security frameworks model data-driven failures.

OWASP Top 10 for LLM Applications

* **LLM04 – Data & Model Poisoning**: Attackers manipulate training data, embeddings, or corpora to influence behaviour.  
* **LLM07 – Insecure Model Monitoring**: Behavioural drift may remain undetected without proper monitoring.  
* **LLM05 – Supply Chain Vulnerabilities**: External data sources and ingestion pipelines expand the attack surface.

NIST AI Risk Management Framework

* **Map**: Identify all data sources that influence model behaviour.  
* **Measure**: Monitor behavioural drift and ranking anomalies.  
* **Manage**: Apply layered controls across ingestion, storage, and monitoring.

EU AI Act

* **Article 9**: Continuous risk management for system behaviour.  
* **Article 10**: Data governance, quality, and lifecycle integrity.

Across these frameworks, poisoning is treated as a system-level integrity failure rather than a model defect.

## **Brief Overview**

Modern AI and RAG systems rely heavily on an **Ingestion Pipeline**, which automatically collects, processes, embeds, indexes, and stores documents into the system’s searchable knowledge base. These pipelines allow AI systems to continuously update information from:

* **Shared Drives**  
* **Internal Documents**  
* **APIs**  
* **Web Sources**  
* **Third-Party Feeds**  
* **Cloud Storage**

An ingestion pipeline commonly performs:

* **Document Collection**  
* **Parsing**  
* **Chunking**  
* **Embedding Generation**  
* **Vector Indexing**  
* **Storage**

Once processed, the content becomes retrievable by the AI model during inference time.

A major security issue is that ingestion pipelines usually trust incoming data automatically. Most systems validate only technical properties such as file type or permissions, but they do not inspect the **semantic intent** of the content. This creates a critical **Trust Boundary** weakness.

Attackers exploit ingestion systems by:

* Uploading malicious documents  
* Modifying trusted files  
* Injecting poisoned content into feeds  
* Exploiting weak parser validation  
* Manipulating automated indexing workflows

Because ingestion is automated, malicious files are processed exactly like legitimate ones. The content becomes:

* Chunked  
* Embedded  
* Indexed  
* Searchable  
* Retrievable

This creates long-term influence over future AI responses.

One important concept is **Automation as an Attack Multiplier**. Automated ingestion pipelines increase speed and scalability, but they also amplify poisoning risks. A single poisoned file may affect thousands of future queries without requiring direct interaction with the model.

A famous real-world example is the **Dependency Confusion Attack (2021)** by Alex Birsan. Major companies such as Microsoft, Apple, Tesla, and JFrog used automated build pipelines that downloaded dependencies from both internal and public repositories.

The attacker uploaded malicious packages using the same names as trusted internal packages. Because the build systems trusted and automatically retrieved dependencies, the malicious packages were installed inside corporate environments without directly breaching the systems.

This demonstrated how:

* **Automation**  
* **Weak Validation**  
* **Implicit Trust**  
* **External Sources**  
* **Pipeline Misconfiguration**

can become major supply chain attack vectors.

Key differences between poisoning layers:

* **Training Data Poisoning** → affects what the model learns  
* **Embedding Poisoning** → affects retrieval ranking  
* **Ingestion Pipeline Attacks** → affect what enters the system initially

Important security risks include:

* **Knowledge Base Poisoning**  
* **Persistent Retrieval Manipulation**  
* **Automated Trust Exploitation**  
* **Silent Data Propagation**  
* **Context Manipulation**  
* **Supply Chain Abuse**  
* **Semantic Injection**  
* **Untrusted Content Indexing**

Important keywords and concepts:

* **Ingestion Pipeline**  
* **Trust Boundary**  
* **Document Parsing**  
* **Chunking**  
* **Embedding Generation**  
* **Vector Indexing**  
* **Automated Ingestion**  
* **Knowledge Base Poisoning**  
* **Dependency Confusion**  
* **Semantic Intent**  
* **Retrieval Manipulation**  
* **Persistent Poisoning**  
* **Supply Chain Security**  
* **Automation Risks**  
* **RAG Security**

---

What type of pipeline collects, parses, and indexes documents into an AI system's knowledge base?

Answer: Ingestion pipeline

In the 2021 dependency confusion attack, what did the build system automatically pull from public repositories?

Answer: Malicious packages

## **Behavioral Poisoning Effects Overview**

Poisoning attacks in AI systems usually do not cause crashes or visible technical failures. Instead, they change how the model behaves over time. The system still produces fluent, logical, and confident responses, making the attack difficult to detect.

There are two major behavioural effects:

• **Obvious Poisoning Effects**  
 These include **persona shifts**, **backdoor triggers**, and extremely incorrect outputs. They are easier to notice because the behaviour visibly differs from the normal system response.

• **Subtle Poisoning Effects**  
 These are more dangerous because they appear normal. The model may:

* Slightly favor one product  
* Quietly modify recommendations  
* Change regulatory thresholds  
* Omit critical security warnings  
* Reframe information in misleading ways

The key issue is **behavioral drift**. Since LLMs are **probabilistic systems**, outputs naturally vary, making it difficult to separate malicious influence from normal variation.

A major real-world example is the **Waze Traffic Data Poisoning** incident, where fake GPS reports and false traffic incidents manipulated routing behaviour. The infrastructure remained operational, but the system’s understanding of traffic became corrupted.

Key Security Concepts:

* **Training Data Poisoning** → Changes what the model learns  
* **Embedding Poisoning** → Changes retrieval relevance  
* **Ingestion Abuse** → Changes what enters the system  
* **Behavioral Drift** → Slow change in model behaviour over time  
* **Trust Degradation** → Users lose confidence in AI outputs  
* **Upstream Manipulation** → Attack occurs before inference

Main Takeaway:  
 The most dangerous poisoning attacks are often the least visible. The model behaves normally on the surface while gradually producing manipulated outputs that influence decisions, recommendations, and trust.

---

What type of poisoning effect causes small, gradual behavioural shifts that appear normal?

Answer: Subtle

When poisoning alters behaviour but the infrastructure remains operational, what has been corrupted?

Answer: data

## **Poisoning Detection and Defense Overview**

Poisoning attacks are difficult to detect because they rarely cause technical failures. The infrastructure, embeddings, and retrieval systems continue operating normally while the model’s behaviour slowly changes over time. This gradual change is known as **behavioral drift**.

Unlike traditional attacks, poisoning targets **trust**, **relevance**, and **data integrity** instead of system code. Since LLMs naturally generate varied responses, identifying malicious influence requires observing long-term behavioural patterns rather than isolated outputs.

Key Areas Where Poisoning Can Occur:

* **Training Data**  
* **Ingestion Pipelines**  
* **Vector Databases**  
* **Retrieval Ranking**

Because poisoning can happen across multiple layers, **layered security controls** are required.

Important Defensive Measures:

* **Source Verification** → Ensures uploaded content comes from trusted origins  
* **Access Control Restrictions** → Prevents unauthorized modifications  
* **Structured Content Review** → Validates data before indexing  
* **Logging and Change Tracking** → Detects unusual document updates  
* **Behavioral Monitoring** → Observes long-term output changes

A major concept in poisoning defense is **Behavioral Drift Monitoring**, which focuses on:

* Tone or persona shifts  
* Recommendation bias  
* Changes before and after ingestion updates  
* Gradual manipulation patterns

Real-World Example: **Amazon Fake Reviews**  
 Amazon experienced large-scale fake review poisoning campaigns where attackers manipulated product rankings using coordinated positive and negative reviews. The fake reviews blended with legitimate activity, making detection difficult.

Amazon responded using:

* **Machine Learning Detection**  
* **Anomaly Monitoring**  
* **Human Investigation**  
* **Identity Restrictions**  
* **Legal Enforcement**  
* **Ranking Corrections**

Between 2023–2024, Amazon reported blocking over **250 million** suspected fake reviews before publication.

Main Takeaway:  
 There is no single tool that fully stops poisoning attacks. Effective AI security depends on **defense-in-depth**, combining technical controls, monitoring, governance, and continuous review of what the model learns from and retrieves.

---

What type of monitoring focuses on detecting gradual changes in model outputs over time?

Answer: Behavioural Monitoring 

In the Amazon fake reviews case study, approximately how many suspected fake reviews did Amazon report blocking before publication in 2023-2024?

Answer: 250 million

## **Poisoned Knowledge Base Overview**

This scenario demonstrates how an AI assistant can be manipulated without changing the model itself. Instead of attacking the model weights or prompts, the attacker modifies the system’s **trusted reference material**.

The AI assistant relies on an internal **knowledge base** to answer employee questions. Since any authorised user can update the references without validation or review, the attacker injects a fake password reset policy containing weaker security controls.

The poisoned update introduced:

* **180-day password rotation** instead of 90 days  
* Reduced password complexity requirements  
* Removal of special characters and number requirements  
* Use of an external password reset portal  
* Removal of multi-factor verification steps

After ingestion, the assistant treated the malicious content as legitimate because retrieval systems trust indexed data by default.

Important Security Concepts:

* **Inference-Time Poisoning** → Behaviour changes during retrieval, not training  
* **Knowledge Base Manipulation** → Altering trusted stored information  
* **Context Injection** → Retrieved content influences model outputs  
* **Behavioral Drift** → The assistant gradually changes responses  
* **RAG Security Risk** → Retrieval systems cannot verify truth or intent  
* **Trust Boundary Failure** → Unvalidated content becomes authoritative

Key Observation:  
 The deployment process response remained unchanged because only the password policy documents were poisoned. This proves the attack targeted the retrieval layer rather than the entire model.

Main Takeaway:  
 In RAG-based systems, attackers do not need direct access to the model. Controlling what the system retrieves is often enough to manipulate outputs, weaken security guidance, and influence user decisions.

---

Which component was modified to influence the assistant’s behaviour?

Answer: Reference Material

What attack class does modifying trusted knowledge fall under?

Answer: Data Poisoning

**Conclusion**

Data and model poisoning change how trust operates in AI systems by targeting what the model learns and what it retrieves. Instead of attacking prompts or application code, poisoning manipulates the data layer. When that layer is compromised, the model behaves differently while still appearing to function normally. In this room, you learned that poisoning can occur at multiple stages: training data, embedding and corpus storage, and ingestion pipelines. These attacks do not require direct access to model weights or user prompts. By influencing trusted data sources, attackers can create subtle behavioural drift or obvious manipulation at scale. Poisoning is dangerous because it targets assumptions about data integrity. The model continues to operate as designed. The failure lies in what it was allowed to learn or retrieve.

## Key Takeaways

* Control over data can equal control over behaviour  
* Embedding and ranking manipulation can influence outputs without retraining  
* Automation amplifies poisoning risk at scale  
* Subtle behavioural drift is often more dangerous than obvious failure  
* No single detection mechanism is sufficient

## Framework Alignment

The risks explored in this room align with how modern AI security frameworks model data-driven failures.

OWASP Top 10 for LLM Applications

* **LLM04 – Data & Model Poisoning**: Attackers manipulate training data, embeddings, or corpora to influence behaviour.  
* **LLM07 – Insecure Model Monitoring**: Behavioural drift may remain undetected without proper monitoring.  
* **LLM05 – Supply Chain Vulnerabilities**: External data sources and ingestion pipelines expand the attack surface.

NIST AI Risk Management Framework

* **Map**: Identify all data sources that influence model behaviour.  
* **Measure**: Monitor behavioural drift and ranking anomalies.  
* **Manage**: Apply layered controls across ingestion, storage, and monitoring.

EU AI Act

* **Article 9**: Continuous risk management for system behaviour.  
* **Article 10**: Data governance, quality, and lifecycle integrity.

Across these frameworks, poisoning is treated as a system-level integrity failure rather than a model defect.

