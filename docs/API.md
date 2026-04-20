# 🔌 API Design

## 🧠 Product

**DocuMind AI — Enterprise GenAI Knowledge Assistant**

---

## 1. Overview

The system exposes REST APIs for:

* Document management
* Query processing
* User interaction

All APIs are designed to be:

* Stateless
* Secure
* Scalable

---

## 2. Authentication (Simplified MVP)

* Token-based authentication (JWT or API key)
* Role-based access:

  * Admin
  * User

---

## 3. API Endpoints

---

## 3.1 Upload Document

### Endpoint

POST /upload

### Description

Uploads a document and triggers processing.

### Request

* Content-Type: multipart/form-data

```json
file: PDF file
```

---

### Response

```json
{
  "message": "Upload successful",
  "document_id": "doc_123"
}
```

---

### Errors

* 400 → Invalid file format
* 500 → Upload failure

---

## 3.2 Get Documents

### Endpoint

GET /documents

### Description

Fetch all uploaded documents.

---

### Response

```json
[
  {
    "document_id": "doc_123",
    "name": "policy.pdf"
  }
]
```

---

## 3.3 Delete Document

### Endpoint

DELETE /document/{id}

### Description

Deletes a document and its embeddings.

---

### Response

```json
{
  "message": "Document deleted"
}
```

---

## 3.4 Query System

### Endpoint

POST /query

### Description

Processes user query using RAG pipeline.

---

### Request

```json
{
  "query": "What is the leave policy?"
}
```

---

### Response

```json
{
  "answer": "Employees are entitled to 20 days leave...",
  "sources": [
    {
      "document_id": "doc_123",
      "chunk_id": "c2"
    }
  ]
}
```

---

### Errors

* 400 → Empty query
* 500 → LLM or retrieval failure

---

## 4. API Flow

### Upload Flow

```text
Client → API Gateway → Lambda → S3 → Processing → Qdrant
```

---

### Query Flow

```text
Client → API Gateway → Lambda → Qdrant → Bedrock → Response
```

---

## 5. Validation Rules

* File size limit (e.g., 5MB)
* Only PDF allowed
* Query must not be empty
* Input sanitization

---

## 6. Error Handling Strategy

* Return structured JSON errors
* Log all failures
* Graceful fallback for LLM errors

---

## 7. Rate Limiting (Future)

* Limit requests per user
* Prevent abuse

---

## 8. Future Enhancements

* Streaming responses
* Batch document upload
* Advanced filtering

