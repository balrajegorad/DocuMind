# 🏗️ System Architecture

## 🧠 Product

**DocuMind AI — Enterprise GenAI Knowledge Assistant**

---

## 1. Architecture Overview

DocuMind is a **single-tenant, serverless GenAI system** deployed in each company's AWS account.

The system follows a **Retrieval-Augmented Generation (RAG)** architecture combined with **event-driven processing**.

---

## 2. Core Components

### 2.1 API Layer

* AWS API Gateway
* Handles incoming requests (upload, query)

---

### 2.2 Compute Layer

* AWS Lambda
* Executes business logic
* Handles:

  * Document upload
  * Query processing
  * Orchestration

---

### 2.3 Storage Layer

* AWS S3

  * Stores uploaded documents
* DynamoDB

  * Stores metadata (users, documents)

---

### 2.4 AI Layer

* Amazon Bedrock

  * Embedding generation
  * LLM response generation

---

### 2.5 Vector Database

* Qdrant

  * Stores document embeddings
  * Performs semantic search (top-k retrieval)

---

### 2.6 Infrastructure Layer

* Terraform

  * Automates deployment in client AWS accounts

---

## 3. High-Level Architecture Flow

### 3.1 Document Ingestion Flow

1. Admin uploads document via API
2. File stored in S3
3. Lambda triggered for processing
4. Document parsed and chunked
5. Embeddings generated using Bedrock
6. Embeddings stored in Qdrant with metadata

---

### 3.2 Query Flow (RAG)

1. User sends query
2. Query converted into embedding
3. Qdrant retrieves top-k relevant chunks
4. Context + query sent to LLM (Bedrock)
5. LLM generates final answer
6. Response returned with source references

---

## 4. Multi-Tenant Design

DocuMind uses a **single-tenant deployment model**:

* Each company has its own AWS account
* Separate:

  * S3 storage
  * DynamoDB tables
  * Qdrant instance
* No shared infrastructure

👉 Ensures complete data isolation and security

---

## 5. Security Design

* IAM roles for service access
* No public access to S3 (signed URLs only)
* Role-based access control (Admin/User)
* Metadata filtering in vector database
* No cross-tenant data access

---

## 6. Scalability Strategy

* Lambda auto-scales based on requests
* S3 provides unlimited storage
* Qdrant scales via container deployment
* API Gateway handles high traffic

---

## 7. Key Bottlenecks

* LLM latency (2–5 seconds per request)
* Embedding generation cost
* Retrieval accuracy (depends on chunking)

---

## 8. Trade-offs

### Serverless vs EC2

* Serverless → easy scaling, less management
* EC2 → more control, but higher maintenance

---

### Single-Tenant vs Multi-Tenant

* Single-tenant → high security
* Multi-tenant → lower cost

---

### Chunk Size

* Smaller → better accuracy
* Larger → better context

---

## 9. Failure Handling

* If LLM fails → return fallback response
* If parsing fails → return raw text
* If retrieval fails → return partial answer

---

## 10. Future Enhancements

* Multi-tenant SaaS model
* Advanced retrieval (re-ranking)
* Caching layer (Redis)
* Streaming responses

