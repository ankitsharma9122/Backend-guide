# Idempotency

> **An operation is idempotent if performing it multiple times produces the same final system state as performing it once.**

### Why is it required?

Idempotency makes **retries safe** in distributed systems and prevents duplicate side effects caused by:

* Network failures
* Client retries
* Load balancer retries
* Timeouts
* User double-clicks

### How to implement it?

Use a **stable business identifier** when one already exists:

```text
orderId
transactionId
bookingId
UTR
```

If no stable identifier exists, use an **Idempotency Key**.

```text
Client
  ↓
POST /orders
Idempotency-Key: abc123
  ↓
Server
  ↓
Check key/orderId
  ↓
Already processed?
 ┌───────────────┐
 │ Yes           │ No
 ↓               ↓
Return previous  Process request
result           + store result/key
```

**Rules:**

* Same operation retry → **same key**
* New operation → **new key**
* Store the key/result in **Redis or DB**
* Prefer a **unique DB constraint** as the final protection against duplicates.

### Example

Client sends:

```text
POST /orders
orderId = 123
```

Server successfully creates the order, but the response is lost.

Client retries:

```text
orderId = 123
```

Server detects that `orderId = 123` was already processed and returns the **previous result** instead of creating another order.

> **Retry = client sends the request again.**
> **Idempotency = server guarantees the retry does not create duplicate side effects.**

---

## Which HTTP methods are idempotent?

| Method | Idempotent? | Reason                                         |
| ------ | ----------- | ---------------------------------------------- |
| GET    | ✅           | Repeated requests don't change state           |
| PUT    | ✅ Usually   | Replaces resource with the same representation |
| DELETE | ✅           | Repeated deletion leaves the resource deleted  |
| POST   | ❌ Usually   | Can create a new resource/effect each time     |
| PATCH  | ⚠️ Depends  | Depends on the operation                       |

Example:

```text
PATCH { "status": "ACTIVE" }
→ Idempotent

PATCH { "$inc": { "count": 1 } }
→ Non-idempotent
```

### Important points

**JWT does NOT automatically make a request idempotent.**

Idempotency depends on the **operation's side effects**, not whether authentication uses JWT.

**Login is not automatically idempotent either.**

For example, if every login creates a new session/token:

```text
Login → Session A
Retry → Session B
```

the operation has different side effects and is therefore **not idempotent**.

> **Idempotency is primarily about the final state/side effects of the syste**
