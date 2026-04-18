# Data & Storage Vocabulary

## SQL
Structured Query Language — relational databases with schemas, ACID guarantees, joins.
Best for: structured data, complex queries, transactions.
Tools: [[PostgreSQL]], [[MySQL]].
Related: [[ACID]], [[NoSQL]], [[normalization]]

## NoSQL
Non-relational databases — flexible schemas, horizontal scaling, various data models.
Types: document ([[MongoDB]]), key-value ([[Redis]]), column-family (Cassandra), graph ([[graph-database]]).
Best for: flexible schemas, high write throughput, simple access patterns.
Related: [[SQL]], [[eventual-consistency]], [[sharding]]

## graph-database
Database optimised for storing and querying highly connected data.
Best for: social networks, recommendation engines, fraud detection.
Tools: Neo4j, Amazon Neptune.
Related: [[NoSQL]], [[SQL]]

## time-series-database
Optimised for time-stamped data — metrics, events, sensor readings.
Best for: monitoring, IoT, financial tick data.
Tools: InfluxDB, TimescaleDB, Prometheus.
Related: [[observability]], [[NoSQL]]

## eventual-consistency
System guarantees that if no new updates, all replicas will eventually converge.
Trade-off: higher availability, lower latency, but reads may be stale.
Related: [[CAP-theorem]], [[consistency]], [[BASE-theorem]]

## ACID
Atomicity, Consistency, Isolation, Durability — guarantees for database transactions.
Atomicity: all-or-nothing. Consistency: valid state always. Isolation: concurrent tx don't interfere.
Related: [[SQL]], [[BASE-theorem]], [[PostgreSQL]]

## BASE-theorem
Basically Available, Soft state, Eventually consistent.
The distributed systems counterpart to [[ACID]] for [[NoSQL]] systems.
Related: [[ACID]], [[eventual-consistency]], [[CAP-theorem]]

## normalization
Organising relational data to reduce redundancy (1NF, 2NF, 3NF, BCNF).
Higher normal forms → less duplication, more joins needed.
Related: [[SQL]], [[indexing]]

## indexing
Data structure (B-tree, hash, etc.) that speeds up query lookups at the cost of write speed and storage.
Types: primary, secondary, composite, partial, full-text.
Related: [[SQL]], [[query-optimization]], [[NoSQL]]

## query-optimization
Techniques to make database queries faster: indexing, query rewriting, execution plan analysis.
Tools: EXPLAIN ANALYZE in [[PostgreSQL]], slow query logs.
Related: [[indexing]], [[SQL]]

## connection-pooling
Reusing database connections across requests instead of creating a new one each time.
Critical for high-concurrency applications.
Tools: PgBouncer ([[PostgreSQL]]), HikariCP (Java).
Related: [[SQL]], [[scalability]]
