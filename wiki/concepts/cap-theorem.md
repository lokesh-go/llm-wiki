---
t: concept
cat: system-design
tags: [CAP-theorem, consistency, availability, partition-tolerance]
src: 1
rel: [consistency, availability, partition-tolerance, eventual-consistency]
upd: 2026-04-18
---
> A distributed database can guarantee only 2 of 3 properties: Consistency, Availability, and Partition Tolerance.

## Definition
The CAP theorem states that in a distributed system with replicated data, it is impossible to simultaneously guarantee Consistency, Availability, and Partition Tolerance. Since network partitions are a practical reality, the real trade-off is always between **CP** (consistency + partition tolerance) and **AP** (availability + partition tolerance).

## The Three Properties

**[[consistency]]** — All nodes see the same data at the same time. Every read returns the most recent write.

**[[availability]]** — Every request gets a non-error response from any node. Does not guarantee the response contains the most recent write.

**[[partition-tolerance]]** — The system continues operating despite communication breakdowns (lost/delayed connections) between nodes.

## Why You Can't Have All Three

Classic three-node example (A=App, B=DB node-1, C=DB node-2):
- B and C are in sync → Consistency ✅ and Availability ✅ both work
- When a network partition splits B from C → writes to B don't reach C
- Now: serve stale data from C (AP) OR block C until resync (CP)
- **Conclusion:** Partition Tolerance always forces a choice between C and A

## Possible Combinations

| Mode | Guarantees | Sacrifice | When |
|------|-----------|-----------|------|
| **CP** | Consistency + Partition Tolerance | Availability | Shut down non-consistent node during partition |
| **AP** | Availability + Partition Tolerance | Consistency | Serve stale data, resync after partition resolves |
| **CA** | Consistency + Availability | Partition Tolerance | No partitions allowed — impractical in real distributed systems |

## What To Choose In Practice
Partition Tolerance is unavoidable in distributed systems. Always assume it. The real decision is:
- Need strong consistency (banking, inventory)? → Choose **CP** (e.g. [[MongoDB]])
- Need high availability (social feeds, search)? → Choose **AP** (e.g. Cassandra, [[CouchDB]])

## When NOT to Ignore
- Financial transactions → CP required; stale reads are unacceptable
- Leader elections → CP required
- User-facing read-heavy systems → AP often acceptable with [[eventual-consistency]]

## Related Concepts
See also: [[consistency]], [[availability]], [[partition-tolerance]], [[eventual-consistency]], [[BASE-theorem]], [[replication]]

## Sources
- [[cap-theorem]]: CP vs AP trade-off via three-node partition example
- [[what-is-cap-theorem]]: referenced in query "what is cap theorem?"
