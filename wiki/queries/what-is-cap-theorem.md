---
t: query
cat: system-design
tags: [CAP-theorem, consistency, availability, partition-tolerance, eventual-consistency]
src: 0
rel: [cap-theorem]
upd: 2026-04-18
---
> Answer to: "what is cap theorem?"

## Answer

**CAP Theorem** states that a distributed database can guarantee only **2 of these 3 properties** simultaneously:

| Property | What it means |
|----------|--------------|
| **Consistency** | Every read returns the most recent write — all nodes see the same data |
| **Availability** | Every request gets a response from any node (may not be the latest data) |
| **Partition Tolerance** | System keeps working despite network failures between nodes |

### Why you can't have all three

Imagine 3 nodes: App → DB-node-1 ↔ DB-node-2

When a **network partition** splits DB-node-1 from DB-node-2, data sync breaks. You're forced to choose:
- Block DB-node-2 until it resyncs → you gave up **Availability** (CP)
- Serve DB-node-2's stale data → you gave up **Consistency** (AP)

There's no third option.

### The Real Choice

In practice, **partition tolerance is unavoidable** in distributed systems. So the actual decision is always:

| Mode | Choose when | Example |
|------|------------|---------|
| **CP** | Strong consistency is critical (banking, inventory) | [[MongoDB]] |
| **AP** | High availability matters more (social feeds, search) | Cassandra |

**AP systems** use [[eventual-consistency]] — data becomes consistent after the partition heals.

## Pages Consulted
[[cap-theorem]]

## Confidence
high

## Follow-up Questions
- What is the difference between CP and AP in practice?
- How does [[eventual-consistency]] work in AP systems like Cassandra?
- When should I choose CP vs AP for my system design?
- How does [[MongoDB]] handle partitions to maintain consistency?
