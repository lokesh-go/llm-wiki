---
title: "CAP Theorem"
url: https://github.com/lokesh-go/notes/blob/main/database/cap/README.md
author: lokesh-go
published: unknown
fetched: 2026-04-18
category: note
status: compiled
---

# CAP Theorem

- It is desirable property of distributed system with replicated data.
- CAP Theorem is a concept that a distributed database system can only have 2 of the 3: _Consistency, Availability and Partition Tolerance_

---

#### Consistency

- A system is said to be consistent if all nodes see the same data at the same time.
- Simply, if we perform a read operation on a consistent system, it should return the value of the most recent write operation.
- It means that, the read should cause all nodes to return the same data, i.e., the value of the most recent write.


#### Availability

- All nodes should be available. All node should be response.
- Availability in a distributed system ensures that the system remains operational 100% of the time.
- Every request gets a (non-error) response regardless of the _individual state_ of a node. (Note: this does not guarantee that the response contains the most recent write.)


#### Partition Tolerance

- A partition is a communications break within a distributed system — a lost or temporarily delayed connection between two nodes.
- Partition tolerance means that the cluster must continue to work despite any number of communication breakdowns between nodes in the system.

---

#### Examples

- MongoDB is a CP data store — it resolves network partitions by maintaining consistency, while compromising on availability.
- Cassandra is an AP database — it delivers availability and partition tolerance but can't deliver consistency all the time. Because Cassandra doesn't have a master node, all the nodes must be available continuously. However, Cassandra provides eventual consistency by allowing clients to write to any nodes at any time and reconciling inconsistencies as quickly as possible.

---

#### Why we can't use CAP together?

Suppose, A, B & C are three nodes.
- A → Application
- B → DB node-1
- C → DB node-2

**Check for Consistency:** B node is sync to C node. So if a=5 at B node then a=5 at C node also. Hence, Consistency is possible.

**Check for Availability:** If A queries B or C node then DB should respond. Hence, Availability is possible.

**Check for Partition Tolerance:** If partition happens between B and C nodes, data sync stops. If a value is updated to 6 at B node, C node won't sync. So data is not consistent. If Partition happens → data is not Consistent. Possible scenario for Partition Tolerance: AP (Availability and Partition Tolerance).

---

#### Possible Cases

- **CA** — Consistency & Availability: can't do a partition between any two nodes.
- **CP** — Consistency & Partition Tolerance: give up Availability. When partition occurs, shut down non-consistent node until partition is resolved.
- **AP** — Availability & Partition Tolerance: give up Consistency. When partition occurs, all nodes remain available but may return older data. Resync after partition resolves.

---

#### What We Should Consider

In distributed systems, Partition Tolerance is a common problem. So we should always consider Partition Tolerance and then choose between Consistency (CP) or Availability (AP).
