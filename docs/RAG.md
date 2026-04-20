# 🧠 RAG Pipeline (Retrieval-Augmented Generation)

## 1. Overview

DocuMind uses a **Retrieval-Augmented Generation (RAG)** pipeline to generate accurate, context-aware answers from internal company documents.

Instead of relying only on the LLM, the system:

* Retrieves relevant document chunks
* Uses them as context
* Generates grounded responses

---

## 2. Pipeline Flow

### Step 1: Document Ingestion

* Upload PDF document
* Extract text
* Clean and preprocess content

---

### Step 2: Chunking Strategy

Documents are split into smaller chunks to improve retrieval accuracy.

* Chunk size: **400–800 tokens**
* Overlap: **50–100 tokens**

👉 Why:

* Smaller chunks → better precision
* Overlap → preserves context

---

### Step 3: Embedding Generation

* Each chunk is converted into a vector embedding
* Generated using Amazon Bedrock embedding model

👉 Purpose:

* Represent semantic meaning of text

---

### Step 4: Storage in Vector Database

* Embeddings stored in Qdrant
* Each chunk includes metadata:

```json
{
  "tenant_id": "company_123",
  "doc_id": "doc_1",
  "chunk_id": "c1"
}
```

👉 Enables:

* Fast retrieval
* Secure filtering

---

### Step 5: Query Processing

* User query is converted into embedding
* Semantic similarity search performed

---

### Step 6: Retrieval (Top-K)

* Retrieve top **k = 3–5** relevant chunks

👉 Trade-off:

* Higher k → better context, more cost
* Lower k → faster, but may miss info

---

### Step 7: Context Construction

* Combine retrieved chunks
* Format into structured prompt

Example:

```text
Context:
[chunk1]
[chunk2]

Question:
{user_query}
```

---

### Step 8: Response Generation

* Send context + query to LLM (Bedrock)
* Generate final answer

---

### Step 9: Source Attribution

* Return:

  * Answer
  * Source document references

👉 Improves trust and transparency

---

## 3. Prompt Design

Prompt rules:

* Do NOT hallucinate
* Answer only from context
* If answer not found → say "Not available in documents"

---

## 4. Key Design Decisions

### Why Chunking?

* Improves retrieval precision

---

### Why Embeddings?

* Enables semantic search beyond keywords

---

### Why Vector DB?

* Efficient similarity search

---

### Why Top-K Retrieval?

* Balances accuracy and cost

---

## 5. Optimization Strategies

* Tune chunk size
* Adjust top-k value
* Reduce token usage
* Improve prompt clarity

---

## 6. Limitations

* Poor chunking → poor answers
* Irrelevant retrieval → hallucination risk
* LLM latency (2–5 seconds)

---

## 7. Future Improvements

* Re-ranking models
* Hybrid search (keyword + semantic)
* Query rewriting
* Caching frequent queries
