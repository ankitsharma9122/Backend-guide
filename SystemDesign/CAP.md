# CAP Theorem (Distributed Systems)

**C – Consistency:** Every client sees the latest data after a write.

**A – Availability:** Every request gets a response, even if it may not have the latest data.

**P – Partition Tolerance:** The system continues to work even if network communication between servers is broken.

### What does Consistency (C) mean?

After a successful write, all replicas return the same, latest value.
