---
t: source
cat: system-design
tags: [CAP-theorem, consistency, availability, partition-tolerance, eventual-consistency]
src: 0
rel: [cap-theorem, consistency, availability, partition-tolerance]
upd: 2026-04-18
---
> CAP Theorem — lokesh-go, unknown. Key topic: CAP theorem in distributed systems with CP vs AP trade-off analysis.

## Summary
- A distributed database can only guarantee 2 of 3: Consistency, Availability, Partition Tolerance
- Partition Tolerance is unavoidable in real distributed systems — always choose between CP or AP
- CP example: MongoDB (consistency over availability during partitions)
- AP example: Cassandra (availability + eventual consistency over strong consistency)
- CA is theoretically possible but impractical — requires no network partition between nodes

## Key Claims
1. Partition Tolerance is a common reality in distributed systems and should always be assumed
2. The real choice is between CP (shut down inconsistent nodes) and AP (serve stale data, resync later)
3. AP systems use eventual consistency — data reconciled after partition resolves

## Concepts Covered
[[CAP-theorem]], [[consistency]], [[availability]], [[partition-tolerance]], [[eventual-consistency]]

## Entities Mentioned
[[MongoDB]], [[Cassandra]]

## My Notes
Personal notes from lokesh-go/notes repo. Good concise explanation of the three-node partition example.
