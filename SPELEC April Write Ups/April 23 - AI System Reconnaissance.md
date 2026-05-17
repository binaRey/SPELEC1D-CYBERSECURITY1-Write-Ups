## **Brief Overview: AI Infrastructure Reconnaissance**

AI infrastructure introduces a much larger and more complex attack surface than traditional networks. Instead of only finding:

* Web servers  
* SSH  
* Databases

You now encounter:

* Model serving frameworks  
* Vector databases  
* AI orchestration platforms  
* Experiment tracking systems  
* LLM runtimes  
* Model registries

These services often:

* Use uncommon ports  
* Expose gRPC endpoints  
* Leak AI metadata  
* Store model artifacts  
* Run without authentication

Reconnaissance becomes critical because exposed AI infrastructure can reveal:

* Models  
* Training pipelines  
* Embeddings  
* Internal datasets  
* Credentials  
* Entire ML environments

## **Key Words / Important Terms to Address**

* **AI Infrastructure**  
* **Model Serving**  
* **Inference Endpoints**  
* **gRPC**  
* **HTTP APIs**  
* **NVIDIA Triton**  
* **TensorFlow Serving**  
* **TorchServe**  
* **Ollama**  
* **vLLM**  
* **MLflow**  
* **Kubeflow**  
* **Ray Dashboard**  
* **Qdrant**  
* **Weaviate**  
* **Milvus**  
* **Jupyter Notebook**  
* **MinIO**  
* **Prometheus Metrics**  
* **RAG Pipeline**  
* **Vector Database**  
* **Embeddings**  
* **Model Registry**  
* **Artifact Storage**  
* **Reconnaissance**  
* **Shodan Dorks**  
* **Cryptomining**  
* **Cleartext Credentials**  
* **Attack Surface**  
* **S3 Buckets**  
* **AI Supply Chain**

## **Important AI Infrastructure Components**

### **Model Serving Endpoints**

Serve AI predictions and LLM inference.

Examples:

* Triton  
* TensorFlow Serving  
* TorchServe  
* Ollama  
* vLLM

### **Common Ports:**

* 8000  
* 8001  
* 8080  
* 8500  
* 11434

### **MLflow**

Tracks:

* Experiments  
* Metrics  
* Models  
* Artifacts

### **Default Port:**

* **5000**

### **Risk:**

Exposes the organization’s entire ML lifecycle.

### **Vector Databases**

Used in:

* RAG systems  
* Semantic search  
* Embedding retrieval

Examples:

* Qdrant  
* Weaviate  
* Milvus  
* Chroma

### **Risk:**

Schema leaks reveal:

* Embedding models  
* Internal datasets  
* Knowledge base contents

### **Jupyter Notebooks**

Used by data scientists for:

* Experiments  
* Training  
* Scripting

### **Default Port:**

* **8888**

### **Major Risk:**

Often exposed without authentication.

### **Prometheus Metrics**

Leak:

* Model names  
* GPU usage  
* Batch sizes  
* Inference latency

### **Recon Value:**

Maps deployment topology silently.

## **Real-World Exposure Risks**

Researchers found:

* Open MLflow dashboards  
* Exposed Jupyter notebooks  
* Open Ray dashboards  
* Public Triton endpoints

Some systems were already compromised with:

* **Cryptominers**  
* **Monero malware**  
* **Credential theft**

## **Main Takeaway**

🔑 AI infrastructure dramatically expands the network attack surface.

A single AI deployment may expose:

* 20+ ports  
* Multiple protocols  
* Sensitive AI metadata  
* Entire ML pipelines

Knowing:

* AI-specific ports  
* Service banners  
* Recon endpoints

is essential for identifying AI systems during assessments.

---

Question: What is the IP address of the host running an HTTP service on port 8888 in your scan results?

Answer: **10.10.45.20**

Question: Which port does MLflow Tracking Server run on by default?

Answer: **5000**

---

# **Brief Overview of AI Service Fingerprinting**

AI service fingerprinting is the process of identifying the actual AI frameworks, inference engines, and machine learning services running behind open ports. Traditional tools like Nmap (-sV) often fail to properly identify AI services because many AI systems use uncommon ports, APIs, and protocols such as HTTP, gRPC, and OpenAI-compatible APIs.

Instead of relying only on port detection, analysts examine:

• HTTP Headers – reveal server identities such as TorchServe, Triton, or Uvicorn  
 • JSON Response Structures – expose framework-specific response formats  
 • Error Messages – leak debugging information, namespaces, tensor structures, and framework names  
 • Endpoint Naming Conventions – AI services commonly use routes like /predict, /infer, /generate, and /embeddings  
 • gRPC Reflection – exposes protobuf schemas and inference service structures  
 • TLS Fingerprinting (JA3/JA4) – identifies automated AI traffic patterns and shared attack infrastructure

Fingerprinting helps determine:

• What AI framework is deployed  
 • Which models are running  
 • Whether services are misconfigured  
 • Possible vulnerabilities and attack surfaces

A major example discussed was the GreyNoise reconnaissance campaign, where attackers used harmless prompts to identify whether targets were using OpenAI, Gemini, Claude, Llama, DeepSeek, or other LLM infrastructures.

---

# **Key Terms and Important Concepts to Address**

## **Core Fingerprinting Techniques**

• HTTP Header Fingerprinting  
 • API Response Signatures  
 • Error Message Fingerprinting  
 • Endpoint Naming Conventions  
 • gRPC Reflection  
 • TLS Fingerprinting (JA3/JA4)

## **Important AI Frameworks / Services**

• TorchServe  
 • Triton Inference Server  
 • TensorFlow Serving  
 • MLflow  
 • FastAPI / Uvicorn  
 • vLLM  
 • LiteLLM  
 • Ollama  
 • Kubeflow  
 • Databricks Mosaic AI

## **Important Headers**

• Server: TorchServe/0.x.x  
 • NV-Status  
 • x-request-id

## **Important Endpoints**

• /predict  
 • /infer  
 • /generate  
 • /embeddings  
 • /v1/models  
 • /v2/models  
 • /api/2.0/mlflow/  
 • /pipeline/apis/v1beta1/

## **Important Tools**

• curl  
 • grpcurl  
 • Nmap  
 • ffuf  
 • feroxbuster

## **Important Security Concepts**

• gRPC Reflection  
 • OpenAI-compatible APIs  
 • Tensor Structures  
 • Telemetry Exposure  
 • Reconnaissance  
 • Attack Surface Enumeration  
 • AI Infrastructure Stack  
 • Reverse Proxy  
 • Binary Protocols  
 • CVE Exploitation Pipelines

## **Important CVEs Mentioned**

• CVE-2024-1558  
 • React2Shell (CVE-2025-55182)

---

# **Why Fingerprinting Matters**

AI systems often expose more debugging and telemetry information than traditional web applications. Misconfigured deployments may leak:

• GPU and CPU usage  
 • Model names and versions  
 • Tensor input/output formats  
 • Internal filesystem paths  
 • Framework namespaces  
 • API schemas through gRPC reflection

This information helps defenders identify insecure deployments, but it can also help attackers map targets for future exploitation.

---

Which unique HTTP response header does the service on 10.10.45.15:8000 return to identify as an NVIDIA product?

Answer: NV-Status

When you run grpcurl against 10.10.45.15:8001, what is the name of the inference service listed in the reflection output?

Answer: inference.GRPCInferenceService

---

# **Brief Overview of AI Service Enumeration**

AI service enumeration is the process of extracting detailed intelligence from identified AI frameworks and infrastructure. While fingerprinting identifies what service is running, enumeration reveals the actual data, models, configurations, storage locations, credentials, and deployment details exposed by those services.

This stage of reconnaissance becomes more serious because attackers and analysts can now gather sensitive operational intelligence from APIs and metadata endpoints.

The lesson focuses heavily on MLflow because it centralizes experiments, models, artifacts, training runs, and deployment metadata in one accessible REST API. Through a few API calls, an attacker can map an organization’s entire machine learning portfolio.

Important information that can be extracted includes:

• Experiment names and project codenames  
 • Registered models and version history  
 • Artifact storage paths such as S3 buckets  
 • User IDs of AI engineers and data scientists  
 • Training metrics and hyperparameters  
 • Tensor configurations and inference formats  
 • Vector database schemas and embedding models  
 • Jupyter notebook credentials and API keys  
 • GPU utilization and inference metrics through Prometheus endpoints

The text also explains how AI services often expose excessive debugging information, auto-generated documentation, and metadata endpoints that unintentionally leak sensitive infrastructure details.

A major example discussed was the IBM X-Force Red attack chain, where attackers moved from a compromised Jupyter notebook to MLflow and then to S3 model storage using exposed credentials stored directly in notebook cells.

# **Key Terms and Important Concepts to Address**

## **Core Enumeration Concepts**

• AI Service Enumeration  
 • REST API Enumeration  
 • Metadata Extraction  
 • Intelligence Gathering  
 • Artifact Enumeration  
 • Infrastructure Mapping  
 • Model Inventory Collection  
 • Deployment Intelligence

## **Important Services and Platforms**

• MLflow  
 • Triton Inference Server  
 • TensorFlow Serving  
 • Weaviate  
 • Qdrant  
 • Chroma  
 • Jupyter Notebook  
 • MinIO  
 • SageMaker

## **Important MLflow Endpoints**

• /api/2.0/mlflow/experiments/search  
 • /api/2.0/mlflow/registered-models/list  
 • /api/2.0/mlflow/model-versions/search  
 • /api/2.0/mlflow/runs/search  
 • /api/2.0/mlflow/artifacts/list

## **Important Triton and TensorFlow Endpoints**

• /v2/models/\<name\>/config  
 • /v1/models/\<name\>/metadata

## **Important Vector Database Endpoints**

• /v1/meta  
 • /v1/schema  
 • /v1/graphql  
 • /collections  
 • /collections/\<name\>

## **Important Jupyter Endpoints**

• /api/kernels  
 • /api/contents

## **Important Data That Can Be Leaked**

• Artifact URIs  
 • S3 Storage Paths  
 • User IDs  
 • Tensor Shapes  
 • Tensor Data Types  
 • Hyperparameters  
 • Git Commit Hashes  
 • GPU Utilization Metrics  
 • Embedding Model Information  
 • Cleartext Credentials  
 • Hugging Face Tokens  
 • API Keys

## **Important Security Concepts**

• Information Leakage  
 • Prometheus Metrics Exposure  
 • Swagger UI Exposure  
 • OpenAPI Enumeration  
 • GraphQL Introspection  
 • RAG Systems  
 • Model Artifact Exfiltration  
 • Credential Exposure  
 • Trust Relationships Between Services

## **Important Tools Mentioned**

• curl  
 • MLOKit  
 • REST APIs  
 • Prometheus  
 • Swagger UI

## **Important Attack Chain Concepts**

• Jupyter Notebook → MLflow → S3 Storage  
 • Credential Harvesting  
 • Model Exfiltration  
 • Model Poisoning  
 • Lifecycle Configuration Abuse

# **Why Enumeration Matters**

Enumeration exposes how AI systems are interconnected and reveals sensitive operational intelligence that may not require exploiting vulnerabilities. Misconfigured AI environments commonly expose internal APIs, model registries, vector databases, notebook files, and storage locations without authentication.

Attackers can use this information to:

• Identify production models  
 • Download model artifacts  
 • Discover internal cloud storage  
 • Steal credentials and tokens  
 • Map deployment infrastructure  
 • Understand AI workflows and RAG pipelines  
 • Prepare for future exploitation or model poisoning attacks

Because many AI platforms prioritize usability and debugging during development, these services often expose far more information than traditional enterprise applications.

# **What MLflow Enumeration Reveals**

MLflow enumeration is especially valuable because it centralizes nearly every stage of the machine learning lifecycle.

Using MLflow APIs, analysts can retrieve:

• Experiment names and IDs  
 • Production and staging models  
 • Artifact storage locations  
 • Model creators and timestamps  
 • Training metrics and parameters  
 • Deployment tags and Git references

This effectively provides a blueprint of an organization’s AI infrastructure.

---

What MLflow REST API endpoint would you use to retrieve the artifact storage location for a specific model version?

Answer: /api/2.0/mlflow/model-versions/search

What is the cleartext password for the MLflow service account stored in the Jupyter notebook on 10.10.45.20?

Answer: Cyphira-MLfl0w-2024\!

---

# **Brief Overview of AI Attack Surface Mapping**

AI attack surface mapping is the process of connecting individual reconnaissance findings into a complete picture of how AI infrastructure communicates, shares data, and exposes vulnerabilities. Instead of viewing services separately, this task focuses on understanding how AI components interact with one another and how one exposed service can lead to the compromise of an entire AI environment.

Modern AI infrastructures contain many interconnected services such as MLflow, Kubeflow, TorchServe, vector databases, Jupyter notebooks, orchestration systems, and cloud storage platforms. These systems continuously exchange data, models, metrics, embeddings, and credentials.

The lesson explains that AI systems greatly expand the traditional attack surface because:

• AI services communicate internally across many ports  
 • Jupyter notebooks often connect to every critical component  
 • Model registries expose production model intelligence  
 • Prometheus metrics leak infrastructure information  
 • Weak network segmentation exposes internal service meshes  
 • Missing mTLS protections allow internal traffic interception

Several dangerous platform misconfigurations were highlighted:

• MLflow shipped without authentication before version 2.x  
 • Kubeflow dashboards are frequently exposed without OIDC authentication  
 • TorchServe allows dynamic loading of malicious .mar files  
 • SageMaker notebooks may allow inbound internet access  
 • Exposed Hugging Face tokens can compromise private AI models

The text also emphasizes supply chain reconnaissance, where attackers identify external AI dependencies such as Hugging Face models, PyPI packages, container builds, and training pipelines. Compromising one dependency can poison the entire ML workflow.

A major focus is the MITRE ATLAS framework, which categorizes AI-related attack techniques and reconnaissance activities. The lesson maps activities such as port scanning, API enumeration, model discovery, and supply chain attacks to specific ATLAS technique IDs.

The ShadowRay campaign was presented as a real-world example where attackers exploited exposed Ray dashboards to gain access to GPU infrastructure, steal credentials, deploy cryptocurrency miners, and establish persistence inside cloud environments.

# **Key Terms and Important Concepts to Address**

## **Core AI Security Concepts**

• AI Attack Surface Mapping  
 • Internal Service Mesh  
 • Lateral Movement  
 • Reconnaissance  
 • Model Enumeration  
 • Metadata Extraction  
 • Credential Harvesting  
 • Supply Chain Reconnaissance

## **Important Platforms and Services**

• MLflow  
 • Kubeflow  
 • TorchServe  
 • Ray Dashboard  
 • SageMaker  
 • Prometheus  
 • Jupyter Notebook  
 • Hugging Face  
 • PyPI

## **Important Vulnerabilities and Misconfigurations**

• Missing Authentication  
 • Exposed Dashboards  
 • Open Management APIs  
 • Weak Network Segmentation  
 • Missing mTLS  
 • Default Credentials  
 • Directory Traversal  
 • Remote Code Execution (RCE)  
 • DirectInternetAccess: Enabled

## **Important CVEs Mentioned**

• CVE-2026-2635  
 • CVE-2026-2033  
 • CVE-2023-48022

## **Important Security Risks**

• Model Artifact Exfiltration  
 • Credential Theft  
 • GPU Resource Hijacking  
 • Dependency Confusion  
 • Supply Chain Poisoning  
 • Cloud Credential Leakage  
 • Unauthorized Model Loading  
 • Cluster-Level Access

## **Important MITRE ATLAS Techniques**

• AML.T0006 – Active Scanning  
 • AML.T0007 – Discover ML Artifacts  
 • AML.T0010 – ML Supply Chain Compromise  
 • AML.T0014 – Discover ML Model Family  
 • AML.TA0002 – Reconnaissance

## **Important Attack Chain Components**

• Jupyter Notebook → MLflow → S3 Storage  
 • Hugging Face Tokens  
 • Malicious .mar Files  
 • Typosquatted PyPI Packages  
 • Environment Variable Dumping  
 • AWS IAM Token Theft

## **Important Real-World Incidents**

• ShadowRay Campaign  
 • IBM X-Force Red Attack Chain

# **Why Attack Surface Mapping Matters**

Attack surface mapping helps defenders understand how individual AI findings connect into a complete compromise path. A single exposed notebook, model registry, or dashboard can provide attackers with credentials, cloud storage access, production model information, and lateral movement opportunities.

Unlike traditional web applications, AI infrastructures contain many interconnected systems that trust each other by default. Once attackers compromise one service, they can often pivot through the rest of the AI ecosystem.

This makes attack surface mapping essential for:

• Identifying high-value AI assets  
 • Understanding trust relationships  
 • Detecting insecure internal communication  
 • Preventing supply chain compromise  
 • Reducing lateral movement opportunities  
 • Securing AI deployment pipelines

---

The Cyphira Jupyter notebook at 10.10.45.20 contains a Hugging Face token, and the MLflow model references a Hugging Face base model. What ATLAS technique ID covers the risk of exposed supply chain dependencies?

Answer: **AML.T0010**

The activities of scanning the subnet with Nmap, probing endpoints with curl, and extracting metadata from MLflow APIs all fall under which overarching ATLAS tactic?

Answer: **AML.TA0002**

---

# **Brief Overview of the 5-Phase AI Reconnaissance Methodology**

The 5-Phase AI Reconnaissance Methodology combines all previous reconnaissance techniques into a structured process used to discover, fingerprint, enumerate, and map AI infrastructure. It also explains how these reconnaissance activities appear inside SIEM logs from a defender’s perspective.

The methodology helps security analysts understand both offensive reconnaissance techniques and defensive monitoring strategies for AI environments.

The five phases include:

• Passive Reconnaissance – gathering public information from sources like Shodan, GitHub dorks, DockerHub, research papers, and job postings  
 • Active Scanning – identifying AI-specific ports and services using tools like Nmap and grpcurl  
 • API Fingerprinting – probing AI endpoints and analyzing headers, JSON structures, error messages, and API routes  
 • Metadata Extraction – enumerating MLflow registries, vector databases, Triton configs, and Jupyter notebooks  
 • Supply Chain Review – identifying exposed Hugging Face tokens, model sources, internal dependencies, and public storage buckets

The lesson emphasizes that reconnaissance activities leave visible traces in SIEM systems. Security teams can detect suspicious behavior such as:

• Repeated requests to `/v2/models`  
 • MLflow API calls without UI sessions  
 • Unauthorized `/metrics` scraping  
 • AI-specific port scanning patterns  
 • Jupyter API access without valid authentication  
 • Path traversal attempts against MLflow artifacts

Several quick defensive improvements were also discussed to reduce the AI reconnaissance surface:

• Enable MLflow authentication  
 • Restrict AI-specific ports from public exposure  
 • Disable insecure Jupyter configurations  
 • Restrict Prometheus metrics access  
 • Rotate and minimize Hugging Face token permissions  
 • Disable Triton model control endpoints  
 • Remove verbose debug messages and headers

A real-world example was the Hugging Face Spaces breach in 2024, where exposed HF tokens allowed attackers to access private models and datasets. This demonstrated how leaked credentials discovered during reconnaissance can later be abused during real attacks.

# **Key Terms and Important Concepts to Address**

## **Core Reconnaissance Phases**

• Passive Reconnaissance  
 • Active Scanning  
 • API Fingerprinting  
 • Metadata Extraction  
 • Supply Chain Review

## **Important AI Security Tools**

• Shodan  
 • Censys  
 • FOFA  
 • GitHub Dorks  
 • Nmap  
 • grpcurl  
 • ffuf  
 • feroxbuster  
 • curl  
 • MLOKit  
 • Nuclei  
 • Agrus Scanner

## **Important AI Services and Components**

• MLflow  
 • Triton Inference Server  
 • TorchServe  
 • Jupyter Notebook  
 • Prometheus  
 • Ray Dashboard  
 • MinIO  
 • Hugging Face  
 • Kubeflow

## **Important Ports Mentioned**

• 5000  
 • 6333  
 • 8000  
 • 8001  
 • 8002  
 • 8080  
 • 8265  
 • 8500  
 • 8501  
 • 8888  
 • 9000  
 • 11434  
 • 19530

## **Important Endpoints**

• /v1/models  
 • /v2/models  
 • /metrics  
 • /graphql  
 • /openapi.json  
 • /api/kernels  
 • /api/contents  
 • /registered-models/list  
 • /model-versions/search

## **Important Detection Indicators**

• Sequential model enumeration requests  
 • MLflow API access without UI sessions  
 • Unauthorized Prometheus scraping  
 • AI-aware port scanning  
 • Jupyter API access without session cookies  
 • Path traversal requests (`../`)

## **Important Security Concepts**

• SIEM Monitoring  
 • Reconnaissance Detection  
 • Attack Surface Reduction  
 • Supply Chain Compromise  
 • Token Exposure  
 • Path Traversal  
 • Model Enumeration  
 • Metadata Leakage  
 • Public Bucket Exposure  
 • Fine-Grained Access Tokens

## **Important Defensive Quick Wins**

• Enable MLflow Authentication  
 • Restrict AI-Specific Ports  
 • Require Jupyter Token Authentication  
 • Disable Triton Model Control  
 • Restrict `/metrics` Access  
 • Rotate Hugging Face Tokens  
 • Audit MinIO and S3 Policies  
 • Remove Debug Headers

## **Important MITRE ATLAS Techniques**

• AML.T0000 – Search for Victim's Publicly Available Research Materials  
 • AML.T0006 – Active Scanning  
 • AML.T0007 – Discover ML Artifacts  
 • AML.T0010 – ML Supply Chain Compromise  
 • AML.T0014 – Discover ML Model Family  
 • AML.TA0002 – Reconnaissance

# **Why This Methodology Matters**

AI infrastructures contain highly interconnected services that expose models, APIs, embeddings, credentials, metrics, and cloud storage paths. A structured reconnaissance methodology allows attackers to systematically identify weaknesses while helping defenders understand how these attacks appear in logs.

Understanding reconnaissance patterns improves:

• Threat detection  
 • SIEM analysis  
 • AI infrastructure hardening  
 • Credential protection  
 • Supply chain security  
 • Exposure management

This methodology also highlights the importance of securing AI development pipelines, model registries, notebooks, and public cloud integrations.

---

A SIEM log shows requests to `/api/2.0/mlflow/registered-models/list` from an IP with no corresponding MLflow UI session. What tool's access pattern does this match?

Answer: **MLOKit**

What is the single most effective quick win for preventing unauthenticated access to the MLflow tracking server?

Answer: **Enable MLflow authentication** 

---

# **Brief Overview of AI Security Framework Mapping**

This section summarizes how AI reconnaissance and attack surface assessment techniques align with major cybersecurity and AI governance frameworks. The goal is to translate technical reconnaissance findings into standardized security language that organizations, auditors, and security teams already recognize.

The activities performed throughout the Cyphira AI Audit — including scanning AI services, fingerprinting frameworks, enumerating APIs, extracting metadata, and identifying supply chain risks — directly map to frameworks such as MITRE ATLAS, ATT\&CK, OWASP Top 10 for LLM Applications, and the National Institute of Standards and Technology AI Risk Management Framework.

These frameworks help classify:

• AI reconnaissance activities  
 • AI infrastructure risks  
 • Model theft exposure  
 • Supply chain compromise risks  
 • Misconfigurations and unauthenticated services  
 • Information leakage from APIs and metrics  
 • AI asset discovery and risk assessment

The room emphasizes that AI security is not separate from traditional cybersecurity. AI environments inherit common network security risks while also introducing new AI-specific threats such as model theft, poisoned dependencies, exposed Hugging Face tokens, unsecured registries, and LLM system enumeration.

# **Key Terms and Important Concepts to Address**

## **Important Frameworks**

• MITRE ATLAS  
 • MITRE ATT\&CK  
 • OWASP Top 10 for LLM Applications (2025)  
 • NIST AI RMF 1.0  
 • NIST CSF 2.0

## **Important MITRE ATLAS Techniques**

• AML.T0000 – Active Scanning  
 • AML.T0048 – Discover ML Artifacts  
 • AML.T0040 – ML Supply Chain Compromise  
 • AML.T0069 – Discover LLM System Information  
 • AML.TA0002 – Reconnaissance

## **Important MITRE ATT\&CK Techniques**

• T1046 – Network Service Scanning  
 • T1592 – Gather Victim Host Information  
 • T1595.002 – Vulnerability Scanning  
 • TA0043 – Reconnaissance

## **Important OWASP LLM Risks**

• LLM03 – Training Data Poisoning / Supply Chain Vulnerabilities  
 • LLM05 – Improper Output Handling  
 • LLM06 – Excessive Agency  
 • LLM10 – Model Theft

## **Important NIST AI RMF Functions**

• Govern  
 • Map  
 • Measure  
 • Manage

## **Important NIST AI RMF Categories**

• Map 1.1 – AI System Component Identification  
 • Map 1.5 – AI Risk Assessment  
 • Map 3.2 – Third-Party AI Resource Risks  
 • Measure 2.6 – AI System Monitoring and Validation

## **Important NIST CSF Categories**

• ID.AM – Asset Management  
 • ID.RA – Risk Assessment

## **Important AI Security Concepts**

• AI Asset Discovery  
 • AI Attack Surface Mapping  
 • Model Enumeration  
 • Metadata Extraction  
 • Supply Chain Reconnaissance  
 • LLM System Enumeration  
 • Model Artifact Exfiltration  
 • Information Leakage  
 • Model Theft  
 • Unauthenticated APIs  
 • Vulnerability Scanning  
 • Infrastructure Risk Assessment

# **Why Framework Mapping Matters**

Framework mapping helps security professionals communicate technical AI reconnaissance findings in a standardized and measurable way. Instead of simply reporting exposed services or leaked tokens, findings can be aligned to recognized frameworks and tactics that organizations already use for compliance, governance, and incident response.

This improves:

• Risk communication  
 • Security reporting  
 • Threat classification  
 • Compliance alignment  
 • AI governance  
 • Security auditing  
 • Incident response planning

By mapping AI findings to frameworks like MITRE ATLAS and NIST AI RMF, organizations gain a clearer understanding of how reconnaissance activities translate into real operational and business risks.

