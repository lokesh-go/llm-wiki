# Architecture Vocabulary

## microservices
Architectural style where an application is a collection of small, independently deployable services.
Each service owns its data and communicates via APIs or events.
Trade-offs: operational complexity, network overhead vs. independent scaling, team autonomy.
Related: [[DDD]], [[API-gateway]], [[service-discovery]], [[event-driven-architecture]]

## monolith
Single deployable unit containing all application functionality.
Types: modular monolith (well-structured), distributed monolith (worst of both worlds).
Often the right starting point before [[microservices]].
Related: [[microservices]], [[strangler-fig]]

## serverless
Executing code without managing server infrastructure. Pay-per-invocation.
Use for: event-driven workloads, sporadic traffic. Avoid for: long-running tasks.
Tools: AWS Lambda, Vercel Functions, Cloudflare Workers.
Related: [[event-driven-architecture]], [[edge-computing]]

## edge-computing
Running compute close to the user, at the network edge.
Reduces latency, good for personalisation and A/B testing at scale.
Tools: [[Cloudflare]] Workers, Vercel Edge, AWS CloudFront Functions.
Related: [[serverless]], [[caching]]

## event-driven-architecture
System design where components communicate through events rather than direct calls.
Decouples producers from consumers. Enables async processing.
Tools: [[Kafka]], [[RabbitMQ]], AWS EventBridge.
Related: [[pub-sub]], [[event-sourcing]], [[CQRS]], [[microservices]]

## hexagonal-architecture
"Ports and Adapters" — isolates core domain logic from external systems (DB, UI, APIs).
Enables easy testing and swapping of infrastructure.
Related: [[clean-architecture]], [[DDD]]

## clean-architecture
Layered architecture: Entities → Use Cases → Interface Adapters → Frameworks.
Dependency rule: inner layers must not depend on outer layers.
Related: [[hexagonal-architecture]], [[DDD]]

## DDD
Domain-Driven Design — aligning software model with business domain language.
Key concepts: bounded contexts, aggregates, entities, value objects, domain events.
Related: [[microservices]], [[CQRS]], [[event-sourcing]]

## CQRS
Command Query Responsibility Segregation — separate read and write models.
Commands change state. Queries read state. Models can be different datastores.
Often paired with [[event-sourcing]].
Related: [[event-sourcing]], [[DDD]], [[event-driven-architecture]]

## event-sourcing
Storing state as a sequence of events rather than current state snapshots.
Full audit log. Enables time-travel debugging. Complex to query.
Related: [[CQRS]], [[DDD]], [[Kafka]]

## saga-pattern
Managing distributed transactions across services using a sequence of local transactions.
Types: choreography (events), orchestration (central coordinator).
Related: [[microservices]], [[event-driven-architecture]], [[DDD]]

## strangler-fig
Migrating a [[monolith]] to [[microservices]] by incrementally replacing parts.
New functionality goes into new services; old functionality migrated over time.
Related: [[monolith]], [[microservices]]

## BFF-pattern
Backend For Frontend — dedicated backend layer per client type (mobile, web, etc.).
Avoids one-size-fits-all API. Each BFF optimised for its client's needs.
Related: [[API-gateway]], [[microservices]]
