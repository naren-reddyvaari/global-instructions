# Performance Guidelines

## 1. General Principles
- Always write clear and simple code first—optimize only when needed.
- Avoid premature optimization; profile before making changes.
- Use performance testing tools (JMH, Gatling, JMeter) to validate improvements.

## 2. Memory Management
- Use primitive types when possible instead of boxed types (int vs Integer).
- Avoid unnecessary object creation inside loops.
- Prefer StringBuilder for heavy string manipulation.
- Reduce usage of large collections; prefer correct data structures.
- Release unused resources (DB connections, streams, file handles) immediately.

## 3. Collections & Data Structures
- Choose the right collection: 
  - ArrayList for fast retrieval
  - LinkedList for frequent insert/delete
  - HashMap for O(1) lookups
- Use ConcurrentHashMap only when needed; it has overhead.
- Avoid synchronized collections unless required.

## 4. Logging Performance
- Avoid expensive operations in log statements.
- Use parameterized logging: `logger.debug("Value is: {}", value);`
- Disable debug logs in production.
- Avoid logging inside tight loops.

## 5. Database Performance
- Use batch inserts/updates.
- Prefer pagination over fetching large datasets.
- Use indexes wisely—avoid over-indexing.
- Avoid N+1 queries; use JOINs or fetch strategies.
- Optimize ORM mappings; prefer DTOs for large reads.

## 6. Caching
- Use caching for frequently accessed data.
- Prefer distributed cache (Redis, Memcached) for shared systems.
- Set proper TTL to avoid stale data.
- Do not cache sensitive and dynamic data unnecessarily.

## 7. API Performance
- Use pagination for APIs returning lists.
- Compress API responses using GZIP.
- Optimize serialization with JSON libraries (Jackson with custom settings).
- Keep DTOs lightweight.

## 8. Concurrency & Threads
- Use thread pools instead of creating new threads.
- Avoid blocking operations in reactive/async flows.
- Prefer CompletableFuture or reactive programming when suitable.
- Ensure thread-safety when working with shared data.

## 9. JVM Tuning Basics
- Use G1GC for most modern applications.
- Increase heap only after understanding memory use.
- Monitor GC logs and heap usage.

## 10. Microservices Performance
- Reduce network chatter; combine calls when possible.
- Use circuit breakers (Resilience4j).
- Use connection pooling for DB and HTTP clients.
- Load test before deploying.

---
These guidelines help ensure a fast, stable, and scalable application across Java and Spring Boot ecosystems.