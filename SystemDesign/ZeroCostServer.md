**If someone wants ₹0 cost when there are zero users/requests, the key is to use serverless / pay-per-use infrastructure instead of always-running servers.**

```text
                 ┌──────────────┐
User ──────────► │ CloudFront   │
                 └──────┬───────┘
                        │
                        ▼
                 ┌──────────────┐
                 │ S3 Static    │
                 │ Website      │
                 └──────┬───────┘
                        │
                        │ API call
                        ▼
                 ┌──────────────┐
                 │ API Gateway  │
                 └──────┬───────┘
                        ▼
                 ┌──────────────┐
                 │ AWS Lambda   │
                 └──────┬───────┘
                        ▼
                 ┌──────────────┐
                 │ DynamoDB     │
                 └──────────────┘
```

### The important difference from EC2

#### EC2 Approach

```text
EC2 Instance
     │
     ├── Running 24 × 7
     ├── CPU/RAM allocated
     └── You pay while it is running
```

Even if:

```text
Users    = 0
Requests = 0
```

the EC2 instance is still running, so there is a cost.

---

### Serverless Approach

With AWS Lambda:

```text
Users = 0
   ↓
Requests = 0
   ↓
Lambda executions = 0
   ↓
No execution-based Lambda charge
```

When a request comes:

```text
Request
   ↓
Lambda starts
   ↓
Process request
   ↓
Execution finishes
```

You pay based largely on **usage**, rather than maintaining an always-running server.

> **Key concept: Scale-to-zero + Pay-per-use architecture.**

**Note:** "₹0" is not guaranteed literally because services such as storage, DNS, or other resources may still incur small charges. The idea is to eliminate the **always-on compute cost**.
