## **Sensitive Information Disclosure in LLM Systems**

Sensitive Information Disclosure falls under **OWASP LLM02** and focuses on protecting **private data**, **confidential records**, **API keys**, and **proprietary information** from being exposed through AI-generated responses. Unlike **prompt injection** or **data poisoning**, this issue does not modify the model or its instructions. Instead, attackers exploit weaknesses in the system’s **retrieval architecture**, especially in **Retrieval-Augmented Generation (RAG)** environments.

In modern AI systems, RAG retrieves information from sources such as **vector databases**, **support tickets**, **internal documents**, and **incident reports** before generating a response. This creates several exposure risks including **over-broad similarity searches**, **shared vector databases**, **weak metadata filtering**, and **unsafe logging systems**. Even without bypassing authentication, confidential information may still leak if the retrieval system fails to enforce proper **tenant isolation** and **access control**.

The scenario demonstrates how an AI assistant accidentally exposed another customer’s **cluster naming convention** because semantically similar support tickets were retrieved from the vector database. The issue was not caused by the AI memorising data, but by insecure **retrieval** and **logging architecture**. This highlights why sensitive information disclosure is dangerous: leaks often appear as normal system behaviour, making them difficult to detect while silently crossing **trust boundaries**.

---

What OWASP category covers sensitive data exposure in LLM systems?

Answer: **LLM02**

## **Sensitive Information Disclosure in RAG Systems**

Sensitive Information Disclosure in **RAG (Retrieval-Augmented Generation)** systems occurs when the architecture improperly handles **retrieved data**, **vector embeddings**, and **logging operations**. Even if the AI model behaves correctly, weaknesses in the surrounding infrastructure can expose confidential information across users, departments, or tenants.

The scenario demonstrates that the issue was not caused by hacking or model failure, but by insecure **retrieval** and **observability architecture**. The system retrieved authorised documents correctly, yet the logging platform stored the complete **augmented prompts** in plaintext. Since the logging environment had broader permissions, confidential security reports became accessible to users who should not have seen them. This created a silent **data boundary failure**.

Several major architectural risks were identified in the discussion:

* **Over-broad retrieval configuration** – retrieving semantically similar documents that may contain unrelated confidential data  
* **Shared vector stores without strict segmentation** – multiple tenants or departments sharing the same retrieval environment  
* **Prompt-based access control instead of database enforcement** – relying on prompts rather than enforcing permissions at the database layer  
* **Logging systems that capture full augmented prompts** – storing sensitive retrieved content inside logs  
* **Treating embeddings as anonymised** – assuming vector embeddings cannot reveal sensitive relationships or information

The **EchoLeak (CVE-2025-32711)** vulnerability further showed how malicious retrieved content could trigger **zero-click prompt injection**, while the **Proof Pudding (CVE-2019-20634)** attack demonstrated that attackers can infer internal model behaviour through repeated output probing and confidence analysis.

---

What mathematical mechanism determines which documents are retrieved in RAG systems?

Answer: **Vector similarity search using embeddings in high-dimensional vector space**

What retrieval parameter controls how many documents are returned?

Answer: **Top-k retrieval setting**

What CVE demonstrated zero-click prompt injection via retrieved content?

Answer: **EchoLeak**

## **Overview: Embedding-Based Retrieval and Security Risks in Modern LLM Systems**

Modern LLM systems frequently use **embedding-based retrieval** to improve knowledge access. Instead of relying on keyword matching, text is transformed into high-dimensional numerical vectors (embeddings) that represent semantic meaning. Similar concepts are positioned close together in vector space, allowing systems to retrieve information based on meaning rather than exact wording.

When a user submits a query, it is also converted into an embedding. The system then performs **similarity search** to find the closest stored vectors and returns the top-ranked documents. These retrieved documents are inserted into the model’s context window, influencing the final response. Importantly, this process is **mathematical, not policy-driven**, meaning relevance is determined by distance in vector space—not authorization rules.

---

## **Key Takeaways**

* Embeddings convert text into numerical vectors representing semantic meaning.  
* Retrieval is based on similarity, not keyword matching or access rules.  
* Cosine similarity and Euclidean distance are commonly used to measure closeness.  
* Top-k retrieval determines what information the model actually “sees.”  
* Small changes in wording can significantly affect retrieval outcomes.  
* Security risks arise because retrieval happens before the model applies reasoning.  
* Embeddings can still expose sensitive information even without plaintext.  
* Vector databases introduce risks like manipulation, inference, and cross-tenant leakage.

---

## **How Similarity Search Affects Data Exposure**

* Similarity search ranks documents based on vector closeness.  
* The most semantically similar documents are retrieved first.  
* Less similar (but potentially safer) documents may never be selected.  
* Retrieval is sensitive to minor semantic or syntactic changes.  
* Visibility in the model context directly determines influence on output.

**Important idea:**  
 If a sensitive document is closer in vector space, it can be retrieved even if it should not be prioritized from a policy perspective.

---

## **Additional Security Risks in Embedding Systems**

### **Corpus Manipulation**

* Malicious or poorly curated documents can be inserted into vector databases.  
* If embeddings are close to common queries, they may repeatedly appear in results.  
* This can subtly influence model outputs over time without retraining.

### **Embedding Inversion**

* Attempts to reconstruct original text from embeddings.  
* Even partial reconstruction may reveal:  
  * Named entities  
  * Sensitive identifiers  
  * Internal project details  
  * Numeric or structured data

### **Membership Inference**

* Determines whether a specific record exists in a dataset.  
* Does not require full reconstruction.  
* Can expose sensitive participation or data presence.

### **Multi-Tenant Vector Store Risks**

* Shared vector databases across users or departments increase exposure risk.  
* Poor filtering may allow cross-tenant retrieval before model-level controls.  
* Isolation failures can lead to unintended data leakage.

---

## **Case Study Insight: Milvus Proxy Vulnerability (CVE-2025-64513)**

* A critical authentication bypass occurred in Milvus Proxy.  
* A user-controlled HTTP header (sourceId) was improperly trusted.  
* Attackers could bypass authentication entirely.  
* Resulting access included:  
  * Reading stored embeddings  
  * Modifying or deleting vectors  
  * Accessing metadata

**Key lesson:**  
 Compromising the vector layer compromises confidentiality, because embeddings themselves encode sensitive relationships—even without raw text.

---

## **Core Idea**

Embedding systems shift the security boundary from **model logic to retrieval mechanics**. Since retrieval is based on mathematical similarity, not authorization rules, sensitive data exposure can occur before the model even processes the prompt.

---

## **Questions and Answers**

What mathematical metric is commonly used to measure similarity between embeddings?

Cosine similarity

What attack attempts to reconstruct text from stored vectors?

Embedding inversion

## **Overview: Retrieval-Augmented Generation (RAG) and Security Boundaries in LLM Systems**

Modern LLM systems extend beyond training data by using **Retrieval-Augmented Generation (RAG)**, where external documents are fetched at inference time and inserted into the model’s context window. This means the model does not only “know” information from training but also dynamically consumes retrieved text during generation. The retrieval step therefore becomes a critical **security boundary**, because whatever is retrieved is directly exposed to the model as input.

Retrieval works by converting a user query into an embedding and performing a similarity search over a vector database. The system selects the top-k closest document chunks based on mathematical distance in vector space, not authorization logic. Once documents enter the context window, the model processes them without awareness of ownership, permissions, or data sensitivity. As a result, retrieval misconfiguration can directly lead to unintended data exposure before any response is generated.

---

## **Key Takeaways**

* RAG injects retrieved documents directly into the model’s prompt at inference time.  
* Retrieval is based on embedding similarity, not access control or authorization rules.  
* The model cannot distinguish between authorized and unauthorized content once it is in the context window.  
* Security failures occur at the retrieval layer, before generation begins.  
* Top-k selection directly controls how much and what type of data is exposed.  
* Metadata filtering must be applied before similarity search to prevent leakage.  
* Stale embeddings can persist and expose outdated or deleted information.  
* Weak retrieval design can lead to cross-tenant or cross-domain data leakage.

---

## **How Retrieval Controls Exposure**

* Queries are embedded into vectors before search begins.  
* Vector databases return the closest matches using similarity metrics.  
* Ranking is purely mathematical (vector proximity), not policy-based.  
* If access control is not enforced early, unauthorized documents can still rank highly.  
* Once retrieved, documents are merged into the prompt and treated as trusted context.  
* The model operates only on visible tokens, without awareness of source restrictions.

---

## **Common RAG Retrieval Failure Modes**

### **Over-Broad Top-k Retrieval**

* Increasing top-k improves context coverage but increases exposure risk.  
* More retrieved chunks \= higher chance of including sensitive or irrelevant data.  
* Even loosely related documents may appear due to vector proximity.  
* All retrieved tokens are treated equally by the model.

### **Weak or Missing Metadata Filtering**

* Metadata includes access level, tenant ID, or classification tags.  
* If filtering is applied after similarity search, unauthorized documents may already be ranked.  
* Prompt instructions cannot enforce database-level security.  
* Filtering must occur before retrieval to be effective.

### **Stale and Persistent Embeddings**

* Deleted or updated documents may remain in vector storage.  
* Outdated embeddings can still be retrieved if not explicitly removed.  
* This can violate compliance requirements in regulated systems.  
* Synchronization gaps lead to unintended exposure of deprecated data.

---

## **System Behavior Insight**

* Retrieval defines what the model “sees.”  
* The model does not validate document authorization.  
* Exposure is determined before generation begins.  
* Security depends heavily on retrieval pipeline design, not just model behavior.

---

## **Case Study Insight: CVE-2025-64513 (Milvus Proxy)**

* Milvus Proxy improperly trusted a user-controlled HTTP header (sourceId).  
* Attackers bypassed authentication entirely by forging the header.  
* Once inside, they could:  
  * Query embeddings  
  * Access metadata  
  * Retrieve cross-collection results  
* The failure was not partial access escalation—it was complete absence of enforcement.  
* When retrieval gateways fail, vector search becomes an unintended disclosure channel.

---

## **Core Idea**

Retrieval is not just a performance optimization in RAG systems—it is the **primary gatekeeper of data exposure**. If retrieval is misconfigured, sensitive information can enter the model context before any reasoning or policy checks occur.

---

## **Questions and Answers**

What retrieval configuration change increases exposure surface by expanding the number of ranked chunks?

Increasing top-k retrieval (returning more nearest neighbor results)

## **Overview: Segmentation and Access Control in RAG-Based Vector Systems**

Retrieval-Augmented Generation (RAG) systems rely on similarity search over vector embeddings rather than identity-based access control. Because of this, **segmentation becomes essential to enforce data boundaries** between users, tenants, and organizational units. Without proper segmentation, vector databases may return results across tenants simply because embeddings are mathematically close, not because access is permitted.

In shared vector environments, retrieval does not inherently understand ownership, permissions, or data classification. It only ranks vectors based on similarity. This makes segmentation and pre-retrieval filtering critical for ensuring that unauthorized data never enters the candidate set. If segmentation is not enforced at the retrieval layer, confidentiality failures occur silently—without errors or alerts—because the system continues to function normally while leaking data through ranking behavior.

---

## **Key Takeaways**

* RAG retrieval is based on similarity, not identity or permissions.  
* Segmentation is required to enforce logical data boundaries.  
* Access control must be applied before similarity search, not after.  
* Shared vector stores can cause cross-tenant data exposure if not properly isolated.  
* Prompt-based restrictions are not sufficient for enforcing security.  
* Vector ranking is influenced even by unauthorized documents if they enter the candidate set.  
* Security in RAG systems depends on retrieval-layer enforcement, not model behavior.  
* Logical separation without enforcement is not true isolation.

---

## **Segmentation in Vector Databases**

### **Per-Tenant Index (Strongest Isolation)**

* Each tenant has a completely separate vector index.  
* Queries only search within that tenant’s dataset.  
* Prevents any cross-tenant retrieval at the architecture level.  
* Provides the strongest security boundary.  
* Higher operational cost due to multiple indexes and synchronization overhead.

### **Per-Role Index**

* Documents are grouped based on access levels (e.g., public, internal, confidential).  
* Query routing depends on user role.  
* Simpler than per-tenant systems but requires strict identity-role consistency.  
* Misconfiguration can allow unauthorized access to restricted indexes.

### **Metadata-Based Filtering (Most Common Approach)**

* All documents exist in a single shared index.  
* Metadata fields (tenant\_id, department, access level) define access rules.  
* Filters must be applied before similarity search.  
* Most flexible approach but also most error-prone.  
* Missing or incorrect filters can result in full dataset exposure.

---

## **Single Shared Index Risk**

* Shared indexes without strict filtering allow cross-tenant retrieval.  
* Similarity search does not respect ownership or organizational boundaries.  
* Even if unauthorized results are removed later, ranking has already been influenced.  
* Exposure occurs during candidate selection, before filtering or response generation.  
* The system behaves as a unified search space unless explicitly constrained.  
* Security depends entirely on query-time restrictions.

---

## **Deterministic Access Control Principle**

* Access control must occur **before similarity computation**.  
* Unauthorized vectors must never enter the candidate pool.  
* Post-retrieval filtering does not prevent ranking influence.  
* Prompt-level instructions are not enforceable security controls.  
* True enforcement happens at the database query layer.  
* If vectors are retrieved, exposure has already begun.

---

## **Case Study Insight: CVE-2024-3033 (AnythingLLM)**

* Internal API endpoints were exposed without authentication.  
* Endpoints allowed direct vector database operations.  
* Attackers could:  
  * Enumerate namespaces  
  * Access workspace structures  
  * Delete or reset vector data  
* Segmentation existed only logically, not in enforcement.  
* Result: cross-workspace visibility and control.  
* Key failure: missing access control at retrieval and management layers.

---

## **Core Idea**

Segmentation in RAG systems is not just organizational—it is a **security enforcement mechanism**. Without strict pre-retrieval access control, similarity search operates on the entire dataset, making data boundaries effectively meaningless.

---

## **Questions and Answers**

What logical grouping inside a vector database separates datasets?

Namespace

Which segmentation model provides the strongest isolation but at a higher cost?

Per-tenant 

What type of enforcement operates before computation instead of after?

Deterministic access control

## **Overview: Defense-in-Depth Strategies for Securing RAG Systems**

In Retrieval-Augmented Generation (RAG) systems, data exposure does not usually result from a single failure but from multiple small weaknesses across the pipeline. Since retrieval, segmentation, logging, and storage all interact, security must be enforced at multiple layers. The core principle is that **confidentiality must be preserved before data reaches the model**, because once sensitive content enters the context window, control over it is effectively lost.

Defense in RAG systems is therefore not a single mechanism but a **layered security model** that reduces exposure risk at ingestion, retrieval, storage, and monitoring stages. Each layer compensates for failures in others, ensuring that no single breakdown leads to full data leakage.

---

## **Key Takeaways**

* Security in RAG systems requires layered defense across the entire pipeline.  
* No single control (filter, policy, or rule) is sufficient on its own.  
* Once sensitive data enters the model context, confidentiality is already compromised.  
* Controls must operate before retrieval, not after generation.  
* Redaction, filtering, segmentation, logging discipline, and monitoring work together as safeguards.  
* Each layer reduces risk but introduces tradeoffs between security, performance, and complexity.  
* Most real-world failures occur due to multiple small misconfigurations combined.

---

## **Defense Mechanisms in RAG Systems**

### **Redaction at Ingestion**

* Sensitive data is removed or masked before embedding creation.  
* Prevents sensitive information from entering the vector database.  
* Techniques include:  
  * Removing PII (personally identifiable information)  
  * Masking secrets or credentials  
  * Replacing sensitive fields with placeholders  
  * Using Named Entity Recognition (NER) before indexing  
* If data is not embedded, it cannot be retrieved later.  
* Tradeoff: aggressive redaction can reduce retrieval accuracy and context quality.

---

### **Retrieval Filtering (Allowlist-Based Control)**

* Enforces strict access rules before similarity search.  
* Filters must be applied at the database query level, not in prompts.  
* Common constraints include:  
  * tenant\_id isolation  
  * role-based access control  
  * exclusion of deleted or archived documents  
* Ensures only authorized documents enter the candidate set.  
* Tradeoff: complex filtering logic increases maintenance and risk of misconfiguration.

---

### **Logging Minimisation**

* Prevents sensitive data from being stored in observability systems.  
* Logs should avoid:  
  * full prompts  
  * retrieved document text  
  * embeddings or raw metadata  
* Safe logging includes:  
  * document IDs  
  * hashed references  
  * structured event summaries  
* Logging sensitive content creates a second uncontrolled copy of the data.  
* Tradeoff: reduced logging can make debugging and incident response harder.

---

### **Data Retention Controls**

* Ensures deleted or outdated data is fully removed from vector storage.  
* Requires synchronized deletion across:  
  * embeddings  
  * indexes  
  * caches  
* Prevents “ghost data” from remaining searchable after deletion.  
* Includes cleanup of:  
  * archived embeddings  
  * temporary indexes  
  * test datasets  
* Tradeoff: increases operational overhead and maintenance complexity.

---

### **Monitoring and Detection**

* Tracks unusual or suspicious retrieval behavior.  
* Key signals include:  
  * spikes in retrieval volume  
  * cross-tenant access attempts  
  * repeated probing of similarity results  
  * sensitive document retrieval patterns  
* Output filtering detects leakage before responses are returned.  
* Tradeoff: high sensitivity may increase false positives and affect usability.

---

## **Core Idea**

RAG security is not enforced by a single safeguard but by a **layered pipeline of controls**. Each stage—ingestion, retrieval, storage, logging, and monitoring—must independently enforce restrictions to prevent sensitive data from reaching the model context window.

---

## **Questions and Answers**

What control removes sensitive data before embedding?

Redaction 

What policy ensures deleted embeddings are removed from storage?

retention

## **Overview: RAG Data Leakage Through Shared Index and Missing Access Controls**

This scenario demonstrates how Retrieval-Augmented Generation (RAG) systems can leak sensitive data when **all documents are stored in a single shared vector index without access control or metadata filtering**. Because retrieval is based on semantic similarity rather than authorization, confidential information can surface during normal user queries.

The failure occurs before generation: once sensitive documents are retrieved into the context window, the model processes them as valid input. Across multiple phases, the system shows how exposure can happen through broad retrieval, logging practices, and semantic overlap in embeddings. Only after enabling access control and metadata filtering are the leaks stopped, proving that **retrieval-layer enforcement is essential for security**.

---

## **Key Takeaways**

* A shared vector index without access control enables cross-data exposure.  
* RAG retrieval is similarity-based, not permission-based.  
* Confidential data can appear during normal queries due to semantic proximity.  
* Logging retrieved documents can create an additional leakage channel.  
* Embedding similarity can cause unrelated sensitive documents to be retrieved (semantic collision).  
* Access control must be enforced before retrieval, not after generation.  
* Metadata filtering restores isolation between public and confidential data.  
* Public functionality can still work even when strict filtering is applied correctly.

---

## **Phase Insights**

### **Phase 1: Overly Broad Retrieval (RAG Retrieval Failure)**

* Normal queries retrieve both public and confidential documents.  
* Salary and CEO compensation data are exposed without authorization checks.  
* Retrieval is driven entirely by vector similarity ranking.  
* No boundary exists between public and sensitive datasets.

---

### **Phase 2: Logging Exposure**

* Retrieval logs contain full document chunks.  
* Confidential content is stored in logs alongside classification tags.  
* Logs become a secondary, persistent exposure point.  
* Even if outputs are filtered, logs still expose sensitive data.

---

### **Phase 3: Semantic Collision (Embedding Overlap)**

* Public query (“employee benefits enrollment”) retrieves unrelated confidential HR data.  
* Cause: embeddings for public and confidential documents are semantically close.  
* Vector proximity leads to incorrect ranking and retrieval.  
* This is known as **semantic collision**, where meaning overlap causes unintended retrieval.

---

### **Phase 4: Access Control Enforcement**

* Metadata filtering is enabled (access control activated).  
* Unauthorized documents are excluded before similarity search.  
* Salary and CEO queries are blocked or restricted to public data only.  
* Public queries continue to function normally.  
* Demonstrates correct retrieval-layer enforcement.

---

## **Core Idea**

RAG systems without access control behave as **unrestricted semantic search engines**, where confidentiality depends on vector distance rather than permissions. Proper security requires filtering and segmentation before retrieval so that sensitive documents never enter the candidate set.

---

## **Questions and Answers**

What caused the assistant to expose confidential data?

Broad Retrieval

Why did Tom Russo's HR record appear when asking about benefits?

Semantic collision

What control could have prevented the disclosure in Phase 2?

Metadata filtering 

## Guarding the Context, Not Just the Model

Everything in this room came back to the same point. LLM systems leak sensitive information without anyone exploiting the model itself. The model didn't choose to disclose anything. The pipeline fed it data it should never have seen, and it did what it always does. It used whatever was in the context window.

Confidentiality in LLM systems is a pipeline problem. Not a model problem. Not a prompt engineering problem. A pipeline problem. If unauthorised data makes it into the context window, the exposure has already happened. It doesn't matter what the system prompt says, what guardrails you added, or how carefully you tuned the output filters. The model saw it. You can't walk that back.

Security has to be enforced before the similarity search runs. Before ranking. Before the context window gets assembled. By the time the model is generating a response, it's too late to decide what it should and shouldn't know.

## Key Takeaways

Sensitive information disclosure in RAG systems usually comes down to a handful of recurring mistakes:

* Top-k retrieval that pulls too broadly and drags in documents from outside the user's scope  
* Metadata filtering that's missing or misconfigured  
* Shared vector indexes where tenants aren't properly isolated from each other  
* Stale embeddings from deleted documents that are still sitting in the index, still searchable  
* Logging systems that record full augmented prompts, creating an unguarded copy of the data  
* Namespace enforcement that exists on paper but breaks under edge cases

Similarity search is math. It finds the closest vectors. That's all it does. It has no concept of who should see what, which department owns a document, or whether something was supposed to be deleted last month.

Authorisation is policy. And if that policy isn't enforced at the vector layer, before the search runs, similarity will happily ignore every boundary you thought you had.

## Red-Team Lessons

When you're assessing an LLM system, forget prompt tricks for a minute. The real findings are in the architecture.

* Can you influence what gets retrieved? Widen the scope, change the filters, see what comes back that shouldn't.  
* Can you trigger similarity results that cross tenant boundaries?  
* Are namespaces enumerable? Can you figure out what other tenants or collections exist just by poking around?  
* Are embeddings directly accessible through the API or some debug endpoint nobody locked down?  
* Are logs capturing full context windows in plaintext somewhere?

The uncomfortable part about disclosure in these systems is that it's quiet. Nothing crashes. No error messages. The system looks stable, responses seem normal, and the whole time it's surfacing sensitive associations that nobody authorised. You won't find it unless you're specifically looking for it.

## Blue-Team Lessons

Defenders need to stop thinking of vector databases as just search infrastructure. They're sensitive storage. Treat them that way.

* Enforce metadata filtering before running similarity searches. Not in the application layer. Not in the prompt. In the query.  
* Apply deterministic namespace isolation to prevent tenants from accidentally seeing each other's data.  
* Remove stale embeddings during retention cycles. If the source document is gone, the embedding should be gone too.  
* Minimise logging exposure. If your logs contain the same content you locked down in retrieval, you've built a side door.  
* Continuously monitor for abnormal retrieval behaviour: volume spikes, cross-tenant access attempts, repeated probing.

The model will not enforce any of this. It can't. It works with whatever lands in the context window and has no idea whether it should be there. The retrieval engine is the last line of defence before the model sees anything. That's where segmentation lives or dies.

## Framework Alignment

This room focused on: **OWASP LLM02 – Sensitive Information Disclosure**

Related OWASP risks include:

* **LLM01** – Prompt Injection (when retrieval trust is abused)  
* **LLM05** – Supply Chain Vulnerabilities (when external corpora are indexed)

From a governance perspective:

* The **NIST AI Risk Management Framework (AI RMF)** emphasises data governance and access control as foundational risk controls.  
* The **EU AI Act** requires appropriate data management and technical safeguards for systems handling sensitive information.

LLM disclosure risk is not theoretical. It is an architectural governance issue.

## Bridge to Upcoming Challenges

In the **next challenge rooms**, you will encounter scenarios where disclosure and poisoning intersect.

You will analyse systems where attackers:

* Inject malicious documents  
* Exploit retrieval boundaries  
* Manipulate embeddings  
* Trigger exfiltration pathways

