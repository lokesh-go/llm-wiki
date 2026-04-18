# System Design Vocabulary

## scalability
Ability to handle increased load by adding resources.
Types: horizontal (add more nodes), vertical (bigger node).
Related: [[load-balancing]], [[sharding]], [[replication]]

## availability
System remains operational and responsive over time.
Measured as uptime %. Trade-off with [[consistency]] (see [[CAP-theorem]]).
Related: [[fault-tolerance]], [[replication]], [[CAP-theorem]]

## consistency
Every read receives the most recent write.
Trade-off with [[availability]] in distributed systems.
Related: [[CAP-theorem]], [[eventual-consistency]], [[ACID]]

## partition-tolerance
System continues operating despite network partitions between nodes.
In [[CAP-theorem]]: always required in distributed systems.
Related: [[CAP-theorem]], [[microservices]]

## CAP-theorem
Distributed systems can guarantee only 2 of 3: Consistency, Availability, Partition-tolerance.
CP systems: [[PostgreSQL]], [[MongoDB]] (with strong consistency).
AP systems: [[Cassandra]], [[CouchDB]].
Related: [[consistency]], [[availability]], [[partition-tolerance]]

## load-balancing
Distributing incoming requests across multiple servers to avoid overload.
Algorithms: round-robin, least-connections, IP-hash, weighted.
Tools: [[Nginx]], [[HAProxy]], AWS ALB.
Related: [[scalability]], [[service-discovery]]

## caching
Storing computed results closer to the requester to reduce latency and load.
Strategies: write-through, write-behind, read-through, cache-aside.
Tools: [[Redis]], [[Memcached]], CDN.
Related: [[scalability]], [[Redis]], [[availability]]

## sharding
Horizontally partitioning data across multiple database nodes.
Each shard holds a subset of the data.
Challenge: cross-shard queries, rebalancing.
Related: [[scalability]], [[NoSQL]], [[replication]]

## replication
Copying data across multiple nodes for redundancy and read scaling.
Types: leader-follower, multi-leader, leaderless.
Related: [[availability]], [[consistency]], [[sharding]]

## fault-tolerance
System's ability to continue operating when components fail.
Achieved via: redundancy, retries, [[circuit-breaker]], failover.
Related: [[availability]], [[circuit-breaker]], [[chaos-engineering]]

## rate-limiting
Controlling how many requests a client can make in a time window.
Algorithms: token-bucket, leaky-bucket, sliding-window-log.
Tools: [[Redis]] (atomic counters), [[API-gateway]].
Related: [[back-pressure]], [[API-gateway]], [[idempotency]]

## back-pressure
Mechanism to slow down producers when consumers are overwhelmed.
Used in: stream processing, message queues, async systems.
Related: [[rate-limiting]], [[Kafka]], [[fault-tolerance]]

## idempotency
Property: applying the same operation multiple times = same result as once.
Critical for: retries in distributed systems, payment APIs, message queues.
Related: [[eventual-consistency]], [[Kafka]], [[REST]]

## circuit-breaker
Pattern that stops calling a failing downstream service after N failures.
States: closed (normal), open (failing, reject calls), half-open (testing).
Prevents cascading failures.
Related: [[fault-tolerance]], [[rate-limiting]], [[microservices]]

## service-discovery
Mechanism for services to find each other's addresses dynamically.
Types: client-side (Eureka), server-side (AWS ALB).
Related: [[microservices]], [[load-balancing]], [[Kubernetes]]

## API-gateway
Single entry point for all client requests, routing to backend services.
Handles: auth, rate-limiting, SSL termination, routing.
Tools: Kong, AWS API Gateway, [[Nginx]].
Related: [[rate-limiting]], [[microservices]], [[load-balancing]]
