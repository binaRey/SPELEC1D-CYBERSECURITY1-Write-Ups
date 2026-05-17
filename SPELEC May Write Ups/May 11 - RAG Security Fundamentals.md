## **Brief Overview**

A **RAG (Retrieval-Augmented Generation)** system improves AI responses by retrieving external documents and supplying them to the **LLM (Large Language Model)** during inference time. Instead of relying only on trained knowledge, the system dynamically pulls information from a connected knowledge source such as databases, documents, or vector stores.

The RAG pipeline works through several connected components:

* **Embedding Model** – Converts text into numerical vectors called **embeddings**.  
* **Vector Store** – Stores embeddings for similarity comparison.  
* **Retriever** – Searches and selects the most relevant documents.  
* **LLM** – Generates the final response using the retrieved context.

The overall workflow follows this sequence:

1. User submits a query  
2. Query becomes an embedding  
3. Vector store compares similarity  
4. Retriever selects matching documents  
5. Retrieved content is injected into context  
6. LLM generates the response

A major security concern in RAG systems is that the model does **not verify** whether retrieved information is:

* Correct  
* Safe  
* Trusted  
* Non-malicious

This creates several AI security risks:

* **Inference-Time Data Poisoning** – Malicious documents influence responses without retraining the model.  
* **Context Manipulation** – Retrieved text changes how the model interprets prompts.  
* **Prompt Injection Through Documents** – Instruction-like text inside retrieved data overrides intended system behaviour.  
* **Retriever Abuse** – Poisoned documents rank highly in similarity searches.

Critical attack surfaces in RAG systems include:

* **Ingestion Layer** – Malicious content entering the knowledge base.  
* **Retrieval Layer** – Unsafe documents being selected.  
* **Context Injection Layer** – Harmful retrieved text influencing generation.

Important defensive strategies include:

* **Document Validation**  
* **Content Filtering**  
* **Trusted Data Sources**  
* **Retriever Constraints**  
* **Prompt Isolation**  
* **Context Sanitisation**  
* **Access Control Policies**  
* **Embedding Monitoring**

RAG security focuses heavily on controlling how external data is retrieved, injected, and processed before it reaches the model.

---

## **Numerical Representation Used in RAG Systems**

Answer: Embeddings

## **Component Responsible for Selecting Documents**

Answer: Retriever

## **Brief Overview**

RAG (**Retrieval-Augmented Generation**) systems introduce new security risks because external documents directly influence how the AI responds during **inference time**. Unlike traditional systems where data is only stored, RAG systems actively retrieve and inject information into the model’s context before generating responses.

The main RAG attack surfaces include:

* **Document Ingestion** – Malicious or untrusted files enter the knowledge base.  
* **Embedding Generation** – Text becomes numerical vectors, hiding important security context.  
* **Similarity-Based Retrieval** – Documents are selected based on semantic similarity rather than trustworthiness.  
* **Context Injection** – Retrieved content is inserted directly into the LLM prompt.

One of the most dangerous stages is **retrieval**, because the process happens automatically and invisibly. The model:

* Cannot verify the document source  
* Cannot identify malicious intent  
* Cannot separate instructions from normal data

During **embedding generation**, important metadata such as:

* **Authorship**  
* **Approval status**  
* **Trust indicators**  
* **Document origin**  
   can be lost. This causes malicious and legitimate content to appear equally valid in vector form.

Attackers exploit this by crafting content that:

* Sounds highly relevant  
* Manipulates similarity scoring  
* Overrides instructions  
* Influences model reasoning  
* Injects hidden prompts

This creates several RAG-specific threats:

* **Inference-Time Data Poisoning**  
* **Semantic Manipulation**  
* **Prompt Injection**  
* **Context Hijacking**  
* **Retriever Abuse**  
* **Knowledge Base Poisoning**

Important security controls for RAG systems include:

* **Strict document validation**  
* **Trusted ingestion pipelines**  
* **Metadata preservation**  
* **Retrieval filtering**  
* **Prompt isolation**  
* **Context sanitisation**  
* **Access controls**  
* **Embedding monitoring**

The most important lesson is that retrieved content should never be treated as automatically trustworthy simply because it was selected by similarity search.

---

## **Highest-Risk RAG Component**

Answer: Retrieval

## **Information Lost During Embedding Generation**

Answer: Context

## **Brief Overview**

**Retrieval Abuse** is a RAG-specific attack where malicious or misleading documents influence AI responses through the retrieval system instead of directly modifying prompts. Attackers exploit how the retriever selects documents and injects them into the model’s context window during inference.

There are two major forms of retrieval abuse:

* **Passive Poisoning** – Malicious documents are quietly inserted into the knowledge base and later retrieved during normal queries.  
* **Active Manipulation** – Attackers intentionally craft content to rank highly for targeted or sensitive searches.

Unlike traditional **prompt injection**, retrieval abuse does not require direct interaction with the model prompt. Instead, attackers manipulate the **retrieval process** itself.

The attack becomes dangerous because retrieved documents are treated as trusted context. The model:

* Cannot verify document intent  
* Cannot identify hidden instructions  
* Cannot distinguish trusted data from malicious content  
* Cannot see retrieval rankings

Retrieval systems select content using **semantic similarity**, meaning documents that “sound relevant” are prioritised, even if they contain:

* False information  
* Hidden instructions  
* Manipulated guidance  
* Unsafe recommendations

This creates several security risks:

* **Context Manipulation**  
* **Inference-Time Poisoning**  
* **Indirect Prompt Injection**  
* **System Intent Override**  
* **Knowledge Base Poisoning**  
* **Semantic Ranking Abuse**

One major problem is that outputs often appear:

* Logical  
* Fluent  
* Well-structured  
* Technically correct

This makes retrieval abuse difficult to detect because the system logs usually show only:

* “Relevant documents retrieved”

The retrieval process appears normal even while the model is being manipulated.

Important defensive measures include:

* **Trusted document ingestion**  
* **Retrieval filtering**  
* **Metadata validation**  
* **Context sanitisation**  
* **Embedding monitoring**  
* **Prompt isolation**  
* **Document trust scoring**  
* **Human review for sensitive outputs**

A critical lesson in RAG security is that **retrieval must be treated as a security boundary**, not only as a search or performance feature.

---

## **Retrieval Abuse Technique Using High-Ranking Malicious Content**

Answer: Active Manipulation

## **Basis Used for Document Selection**

Answer: Semantic Relevance

## **Brief Overview**

RAG (**Retrieval-Augmented Generation**) systems can fail even when responses appear professional, logical, and technically correct. These failures happen because retrieved external content directly influences the model’s reasoning during inference. The model does not verify whether retrieved information is safe, accurate, current, or trustworthy.

Several real-world incidents demonstrated how weaknesses in retrieval pipelines can create major security and governance problems.

### **Microsoft Copilot – Email-Based Retrieval Abuse**

In the **Microsoft 365 Copilot** incident, enterprise emails were used as retrieval sources. Malicious or misleading content hidden inside emails became part of the retrieved context during user queries.

Key issues included:

* **Untrusted ingestion sources**  
* **Context injection**  
* **Implicit trust in retrieved data**  
* **No separation between instructions and information**

This resulted in:

* Exposure of sensitive enterprise information  
* Unsafe AI-generated responses  
* Security restrictions and mitigations

The incident proved that internal company data can still become an attack vector if retrieval is not properly controlled.

### **ChatGPT Plugins – Indirect Prompt Injection**

In the **ChatGPT Plugins** case, external web content and APIs were retrieved dynamically. Some retrieved pages contained hidden instruction-like text that altered the model’s behaviour.

Major weaknesses included:

* **Trusting external sources by default**  
* **No intent validation**  
* **Direct context injection**  
* **Indirect prompt injection**

This caused:

* Manipulated responses  
* Unsafe outputs  
* Redesigns of plugin and retrieval security models

The attack demonstrated how retrieved content can override system intent without changing the actual system prompt.

### **Web-Connected AI Assistants – Stale Retrieval Failures**

Some AI assistants continued retrieving outdated information even after original sources were updated. This was not caused by attackers but by weak governance inside the retrieval pipeline.

Key failures included:

* **No document lifecycle management**  
* **No freshness validation**  
* **Semantic relevance prioritised over accuracy**  
* **Old documents remaining indexed**

This resulted in:

* Incorrect guidance  
* Compliance risks  
* Reduced trust in AI systems

A major lesson from these cases is that retrieval pipelines require strong governance because retrieval systems amplify whatever content they index — whether accurate, malicious, outdated, or misleading.

Important security and governance controls include:

* **Document lifecycle management**  
* **Freshness validation**  
* **Trusted ingestion pipelines**  
* **Retrieval filtering**  
* **Metadata verification**  
* **Context sanitisation**  
* **Access controls**  
* **Human review for sensitive outputs**

The most dangerous aspect of RAG failures is that the system often behaves exactly as designed while still producing harmful or misleading outputs.

---

## **System Area Where Governance Gaps Caused Failures**

Answer: Retrieval Pipeline

## **Brief Overview**

RAG (Retrieval-Augmented Generation) systems improve AI responses by retrieving external documents and injecting them into the model’s context during inference time. While this increases accuracy and relevance, it also creates major **security risks** because the model does not verify whether retrieved content is safe, correct, or trustworthy.

A RAG pipeline typically includes an **Embedding Model**, **Vector Store**, **Retriever**, and **Language Model (LLM)**. The embedding model converts text into numerical **vectors**, while the retriever selects documents using **semantic similarity** instead of trust or correctness. Because of this, malicious or misleading documents can rank highly and influence outputs.

One major risk is **Inference-Time Data Poisoning**, where attackers insert harmful documents into the knowledge base. These documents may contain false information or hidden instructions that manipulate model behaviour without modifying the original prompt. This attack is known as **Retrieval Abuse**.

Two common forms of retrieval abuse are:

* **Passive Poisoning** — malicious documents are inserted once and retrieved later during normal use.  
* **Active Manipulation** — attackers deliberately craft content to rank highly for sensitive or common queries.

The highest-risk area in a RAG system is the **Retrieval** stage because the model cannot:

* Verify document intent  
* Distinguish instructions from data  
* Identify trusted versus untrusted sources

Once retrieved content enters the context window, the model treats it as authoritative information.

Several real-world incidents demonstrated these risks:

* **Microsoft Copilot (2026)** showed how malicious email content could influence AI responses through retrieval.  
* **ChatGPT Plugins (2023)** exposed indirect prompt injection risks from untrusted external content.  
* **Web-Connected AI Assistants** returned outdated information because retrieval prioritised relevance over freshness.

These incidents highlighted critical weaknesses in:

* **Document Ingestion**  
* **Context Injection**  
* **Retrieval Governance**  
* **Freshness Validation**  
* **Output Monitoring**

To reduce risk, organisations use **Layered Defence Architecture**, including:

* **Guardrails** on retrieved content  
* **Validation During Ingestion**  
* **Approval Workflows**  
* **Behavioural Monitoring**  
* **Output Review**  
* **Document Lifecycle Management**

A key detection strategy is monitoring **Output Drift**, which refers to gradual behavioural changes caused by long-term poisoning or misleading data influence instead of a sudden visible attack.

Important keywords and concepts:

* **RAG Security**  
* **Inference-Time Data Poisoning**  
* **Retrieval Abuse**  
* **Context Injection**  
* **Semantic Similarity**  
* **Embedding Vectors**  
* **Output Drift**  
* **Behavioural Monitoring**  
* **Defence-in-Depth**  
* **Indirect Prompt Injection**  
* **Document Governance**  
* **Vector Store**  
* **Retriever**  
* **Knowledge Base Poisoning**

What type of monitoring is a useful way to detect RAG poisoning? 

Answer: Behavioural monitoring

What does output drift reflect instead of a sudden failure? 

Answer: Gradual influence

