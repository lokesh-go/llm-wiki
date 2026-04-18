# Technologies Vocabulary

## Kafka
Distributed event streaming platform. High throughput, durable, ordered within partitions.
Use for: event sourcing, log aggregation, stream processing, high-throughput messaging.
NOT for: low-latency RPC, tiny datasets, simple task queues.
Related: [[RabbitMQ]], [[event-sourcing]], [[pub-sub]], [[back-pressure]]

## RabbitMQ
Message broker implementing AMQP. Flexible routing, push-based, per-message ACK.
Use for: task queues, complex routing, RPC-style messaging.
NOT for: very high throughput, long-term event replay.
Related: [[Kafka]], [[pub-sub]]

## Redis
In-memory data structure store. Extremely fast (sub-millisecond).
Use for: caching, sessions, leaderboards, pub-sub, rate-limiting, distributed locks.
NOT for: primary relational storage, large datasets > RAM.
Related: [[Memcached]], [[caching]], [[pub-sub]], [[rate-limiting]]

## Memcached
Simple, distributed in-memory key-value cache. Multi-threaded, no persistence.
Use for: simple caching of serialised objects. Less features than [[Redis]].
Related: [[Redis]], [[caching]]

## PostgreSQL
Advanced open-source relational database. Full ACID, JSONB, full-text search, extensions.
Best-in-class for: complex queries, transactions, geospatial (PostGIS).
Related: [[SQL]], [[ACID]], [[indexing]]

## MySQL
Popular open-source relational database. Widely supported, simpler than [[PostgreSQL]].
Related: [[SQL]], [[PostgreSQL]], [[ACID]]

## MongoDB
Document-oriented NoSQL database. Flexible BSON/JSON documents.
Use for: flexible schemas, hierarchical data, rapid prototyping.
Related: [[NoSQL]], [[eventual-consistency]]

## Elasticsearch
Distributed search and analytics engine. Built on Lucene. Full-text search, aggregations.
Use for: search, log analysis, autocomplete.
NOT for: primary datastore, strong consistency requirements.
Related: [[NoSQL]], [[indexing]]

## Kubernetes
Container orchestration platform. Manages deployment, scaling, self-healing of containers.
Key concepts: pods, deployments, services, ingress, namespaces.
Related: [[Docker]], [[microservices]], [[IaC]], [[service-discovery]]

## Docker
Platform for building and running containers. Packages app + dependencies into portable images.
Related: [[Kubernetes]], [[microservices]], [[IaC]]

## gRPC
High-performance RPC framework using Protocol Buffers. Strongly typed, streaming support.
Use for: internal service-to-service communication. Faster + smaller than REST+JSON.
Related: [[REST]], [[microservices]]

## GraphQL
Query language for APIs. Clients specify exactly what data they need.
Use for: flexible client needs, BFF layers, reducing over/under-fetching.
Related: [[REST]], [[BFF-pattern]]

## REST
Representational State Transfer — HTTP-based API style using resources and verbs.
Stateless, cacheable, widely understood.
Related: [[GraphQL]], [[gRPC]], [[idempotency]]

## WebSocket
Full-duplex communication protocol over a single TCP connection.
Use for: real-time features (chat, live updates, collaborative editing).
Related: [[REST]]

## Nginx
High-performance HTTP server, reverse proxy, and load balancer.
Use for: static file serving, SSL termination, load balancing, rate limiting.
Related: [[load-balancing]], [[HAProxy]], [[API-gateway]]

## HAProxy
High-availability load balancer and proxy. TCP and HTTP, extremely performant.
Related: [[Nginx]], [[load-balancing]]

## AWS
Amazon Web Services — leading cloud provider. Compute (EC2, Lambda), storage (S3), DB (RDS), etc.
Related: [[GCP]], [[Azure]], [[serverless]]

## GCP
Google Cloud Platform — cloud provider. Strong in AI/ML (Vertex AI), data (BigQuery).
Related: [[AWS]], [[Azure]]

## Azure
Microsoft Azure — cloud provider. Strong in enterprise, .NET, Active Directory.
Related: [[AWS]], [[GCP]]

## Vercel
Platform for frontend deployment. Native Next.js support, edge functions, preview deployments.
Related: [[Next.js]], [[serverless]], [[edge-computing]]

## Cloudflare
CDN, DDoS protection, DNS, and edge compute (Workers). Global network.
Related: [[edge-computing]], [[serverless]], [[rate-limiting]]
