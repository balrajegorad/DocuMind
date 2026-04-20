# ☁️ Infrastructure Design

## 🧠 Product

**DocuMind AI — Enterprise GenAI Knowledge Assistant**

---

## 1. Overview

DocuMind uses a **serverless AWS architecture** deployed in each client’s AWS account using Terraform.

The infrastructure is designed to be:

* Scalable
* Secure
* Isolated per client
* Fully automated

---

## 2. Infrastructure Components

### 2.1 API Layer

* AWS API Gateway
* Handles all incoming HTTP requests

---

### 2.2 Compute Layer

* AWS Lambda
* Executes backend logic
* Handles:

  * Document upload
  * Query processing
  * RAG orchestration

---

### 2.3 Storage Layer

#### S3

* Stores uploaded documents
* Private access only

#### DynamoDB

* Stores metadata:

  * Documents
  * Users
  * Access control

---

### 2.4 AI Services

* Amazon Bedrock

  * Embeddings
  * LLM inference

---

### 2.5 Vector Database

* Qdrant deployed via container (ECS/EC2)
* Stores embeddings
* Handles similarity search

---

### 2.6 Infrastructure as Code

* Terraform used to provision:

  * S3
  * Lambda
  * API Gateway
  * DynamoDB
  * ECS/EC2 for Qdrant
  * IAM roles

---

## 3. Deployment Model

DocuMind follows a **single-tenant deployment model**:

* Each client has its own AWS account
* Infrastructure deployed via Terraform
* No shared services between clients

👉 Ensures:

* Data isolation
* Security
* Compliance

---

## 4. Terraform Workflow

### Step 1: Setup

```text
terraform init
```

---

### Step 2: Plan

```text
terraform plan
```

---

### Step 3: Apply

```text
terraform apply
```

---

### Step 4: Output

* API endpoint
* Service URLs

---

## 5. Security Design

* IAM roles with least privilege
* S3 private buckets
* Signed URLs for access
* Role-based access control
* No cross-tenant data sharing

---

## 6. Scaling Strategy

* Lambda auto-scales based on load
* API Gateway handles concurrent requests
* S3 provides unlimited storage
* Qdrant scales via container replication

---

## 7. Logging & Monitoring

* AWS CloudWatch

  * Logs
  * Metrics
* Track:

  * Errors
  * Latency
  * API usage

---

## 8. Cost Considerations

* Bedrock usage (per request)
* Lambda execution cost
* Storage cost (S3)
* Container cost (Qdrant)

👉 Optimization:

* Reduce token usage
* Limit context size
* Efficient retrieval

---

## 9. Trade-offs

### Serverless vs EC2

* Serverless → scalable, low maintenance
* EC2 → more control

---

### Managed vs Self-hosted Vector DB

* Managed → easier, costly
* Self-hosted (Qdrant) → more control

---

## 10. Future Improvements

* Fully managed vector DB
* Multi-region deployment
* Auto-scaling Qdrant
* CI/CD pipeline integration
