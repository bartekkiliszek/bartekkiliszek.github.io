---
layout: page
title: "Roadmap: Java Senior → Staff Engineer"
permalink: /roadmaps/java-senior/
---

**Format:** Series of focused sprints (4-8 weeks each)  
**Philosophy:** One topic → One resource → One project → Verification  
**Starting point:** 4 years of Java/Spring experience  
**Goal:** Senior/Staff Engineer specializing in Real-Time Trading Systems  

**Progress Legend:** `- [ ]` = Not started | `- [x]` = Completed

---

## How to use this roadmap

### Sprint structure

```
SPRINT X: [Topic name]
━━━━━━━━━━━━━━━━━━━━━━━━
Duration: X weeks
Goal: [What I want to achieve]
Resource: [One book/course/documentation]
Project: [Concrete deliverable]
At work: [How to apply immediately]
Verification: [How I know I've learned it]
```

### Rules

1. **One sprint at a time** – don't start the next one until you finish the current one
2. **Project is mandatory** – no project, no learning
3. **Verification is mandatory** – if you can't explain it, you don't know it
4. **Life will extend it** – 4 weeks often becomes 6-8, that's normal
5. **You can repeat** – if a sprint didn't "click", do it again

### Estimated time

- ~76 sprints
- Average 6-8 weeks per sprint (with life buffer)
- **Total: 8-12 years** (and that's OK)

---

## Table of Contents

1. [PHASE 1: Backend Fundamentals + Integrations](#phase-1-backend-fundamentals--integrations) (Sprints 1-17)
2. [PHASE 2: Java Concurrency & JVM](#phase-2-java-concurrency--jvm) (Sprints 18-29)
3. [PHASE 3: Go + Algorithms](#phase-3-go--algorithms) (Sprints 30-40)
4. [PHASE 4: Architecture and Patterns](#phase-4-architecture-and-patterns) (Sprints 41-52)
5. [PHASE 5: Low-Latency](#phase-5-low-latency) (Sprints 53-64)
6. [PHASE 6: Leadership](#phase-6-leadership) (Sprints 65-73)
7. [Summary](#summary)

---

## Where to start? (First month)

Before you dive into sprints, do these two things:

**Week 1-2:** Run `async-profiler` on your feed handler at work. Generate a flame graph. You don't need to understand everything yet – the point is to see where CPU time actually goes.

**Week 3-4:** Start Sprint 1. If you have an urgent project with REST integration, you can start directly with Sprint 2.

---

# PHASE 1: Backend Fundamentals + Integrations

*Sprints 1-17 | Goal: Solid backend foundations with special emphasis on external system integrations*

---

## SPRINT 1: HTTP & REST – Protocol fundamentals

**Duration:** 4 weeks

**Goal:** I understand HTTP at a deeper level than "GET returns data". I know the protocol from both server and client side.

**Resource:**
- HTTP/REST section at [roadmap.sh/backend](https://roadmap.sh/backend){:target="_blank"}
- RFC 7231 (HTTP Semantics) – skim through, you don't need to read all of it
- "HTTP: The Definitive Guide" – chapters 1-4 (optional)

**Project:**
Design and implement REST API for "Mini Feed Service":
- GET /events (list of sports events)
- GET /events/{id} (event details)
- GET /events/{id}/markets (markets for event)
- Correct HTTP codes (200, 201, 400, 404, 500)
- Pagination (offset/limit or cursor)
- Filtering (sport, league, status)
- Versioning (header or URL)

**At work:**
- Review one of your endpoints – are HTTP codes correct?
- Propose a fix if you find a problem

**Verification:**
- [ ] I can explain the difference between 400, 404, 422, and 500
- [ ] I know when to use PUT vs PATCH vs POST
- [ ] I understand what idempotency is and why it matters
- [ ] I know headers: Content-Type, Accept, Cache-Control, ETag

---

## SPRINT 2: REST API Consumer – External API integration ⭐

**Duration:** 6 weeks

**Goal:** I can write a reliable REST API client that handles errors, retries, timeouts and is easy to maintain.

**Resource:**
- "Release It!" (Michael Nygard) – chapters on stability patterns
- Spring WebClient or OkHttp documentation
- Resilience4j documentation (retry, circuit breaker)

**Project:**
Write a client for an external API (e.g., public sports API or mock):

**Layer 1 – Basics:**
- HTTP client (WebClient/OkHttp)
- Request/response mapping (DTO)
- HTTP error handling (4xx, 5xx)
- Timeout configuration (connect, read, write)
- Request and response logging

**Layer 2 – Resilience:**
- Retry with exponential backoff
- Circuit breaker (Resilience4j)
- Fallback (cache, default response)
- Rate limiting (if API has limits)

**Layer 3 – Production-ready:**
- Health check endpoint for provider
- Metrics (request count, latency, error rate)
- Externalized configuration (URL, timeouts, retry policy)

**At work:**
- Apply these patterns in your integration project
- Add metrics to existing client

**Verification:**
- [ ] My client doesn't crash when API doesn't respond
- [ ] Retry works with backoff (I don't bombard the API)
- [ ] Circuit breaker opens on too many errors
- [ ] I have metrics showing integration health
- [ ] Configuration is external (I can change without recompile)
- [ ] I can explain every design decision

---

## SPRINT 3: REST API Consumer – Advanced patterns ⭐

**Duration:** 4 weeks

**Goal:** I handle edge cases and advanced REST integration scenarios.

**Resource:**
- "Enterprise Integration Patterns" – messaging chapters (adaptable to REST)
- Articles on API versioning, pagination, rate limiting
- OAuth2/API keys documentation

**Project:**
Extend the client from previous sprint:

**Data handling:**
- Pagination handling (automatic fetching of all pages)
- Delta/incremental sync (fetching only changes)
- ETag/If-None-Match for cache validation
- Compression (gzip)

**Authentication:**
- API key management
- OAuth2 token refresh
- Token caching and expiry handling

**Advanced scenarios:**
- Bulk operations (batch requests)
- Long polling (if API supports)
- Webhook receiver (reverse integration)

**Architecture:**
- Adapter pattern (isolating API from domain)
- Data normalization to internal model
- Testing with WireMock

**At work:**
- Implement one advanced pattern in your project

**Verification:**
- [ ] I can handle paginated API
- [ ] I know how to do incremental sync
- [ ] I understand adapter pattern and use it
- [ ] My external DTOs are separated from domain model
- [ ] I have tests with WireMock

---

## SPRINT 4: WebSocket – Real-time integration ⭐

**Duration:** 6 weeks

**Goal:** I can write a reliable WebSocket client for a feed provider.

**Resource:**
- WebSocket RFC 6455 (overview)
- Java WebSocket client documentation (Tyrus, Spring WebSocket, or OkHttp)
- Articles on WebSocket reconnection strategies

**Project:**
WebSocket client for feed provider (or mock):

**Connection management:**
- Connect with authentication
- Automatic reconnection with backoff
- Heartbeat/ping-pong handling
- Connection state machine (CONNECTING, CONNECTED, RECONNECTING, DISCONNECTED)
- Graceful shutdown

**Message handling:**
- Message parsing (JSON/binary)
- Message ordering and sequence numbers
- Duplicate detection
- Backpressure (when messages arrive faster than you process)

**Resilience:**
- Connection timeout
- Stale connection detection (no messages for X seconds)
- Fallback to REST polling
- Multiple server endpoints (failover)

**Stale Data Detection (CRITICAL):**
```
The most dangerous thing is not an error (disconnect), but a "frozen" feed:
- Connection looks OK
- But data stopped flowing
- System thinks everything works
```
- `last_message_timestamp` per provider/per event type
- Alert when no messages for X seconds (configurable threshold)
- Distinguish: no data vs "silence" (e.g., match break)
- Dashboard showing "freshness" of each feed
- Automatic switch to backup provider on stale data

**Observability:**
- Metrics: messages/sec, latency, reconnections
- Metric: `feed_last_update_seconds` (time since last message)
- Connection state logging
- Last message timestamp per provider
- Alert: "Provider X: no data for 30 seconds"

**At work:**
- Apply in feed provider integration
- Add stale data monitoring to existing integrations

**Verification:**
- [ ] My client reconnects automatically
- [ ] I handle heartbeat/ping-pong
- [ ] I have state machine for connection lifecycle
- [ ] I detect stale connection (no data for X seconds)
- [ ] I have alert for "frozen" feed
- [ ] I have metrics showing connection health
- [ ] I can distinguish "no data" from "silence because nothing is happening"

---

## SPRINT 5: Message Queues – RabbitMQ ⭐

**Duration:** 6 weeks

**Goal:** I understand RabbitMQ and can write a reliable consumer.

**Resource:**
- "RabbitMQ in Depth" or official RabbitMQ documentation
- Spring AMQP documentation
- Articles on message acknowledgment and reliability

**Project:**
RabbitMQ consumer for feed events:

**Basics:**
- Connection and channel management
- Queue declaration and binding
- Message consumption
- Manual acknowledgment

**Reliability:**
- Ack modes (auto, manual, reject, nack)
- Prefetch count (how many messages at once)
- Dead Letter Queue (DLQ)
- Retry with backoff (via DLQ or plugin)
- Poison message handling

**Performance:**
- Batch consumption
- Concurrent consumers
- Connection pooling

**Production:**
- Health check
- Metrics (messages/sec, queue depth, consumer lag)
- Graceful shutdown (finish processing)

**At work:**
- Apply in existing RabbitMQ integration

**Verification:**
- [ ] I understand the difference between ack, nack, reject
- [ ] I know how to configure prefetch
- [ ] I have DLQ for failed messages
- [ ] I can handle poison message
- [ ] I have metrics for queue depth and consumer lag

---

## SPRINT 6: Integration Patterns – Common patterns ⭐

**Duration:** 6 weeks

**Goal:** I know patterns common to all types of integrations and can design maintainable integration layer.

**Resource:**
- "Enterprise Integration Patterns" (Gregor Hohpe) – chapters 1-5
- Articles on hexagonal architecture / ports & adapters

**Project:**
Redesign integration layer for "Mini Feed Service":

**Architecture:**
```
┌─────────────────────────────────────────────┐
│                  Domain                      │
│  (Event, Market, Odds - clean objects)      │
└─────────────────────────────────────────────┘
                      ▲
                      │ Port (interface)
┌─────────────────────────────────────────────┐
│              FeedProvider                    │
│  interface FeedProvider {                    │
│    Stream<Event> getEvents();               │
│    void subscribe(Consumer<Event>);          │
│  }                                           │
└─────────────────────────────────────────────┘
        ▲                ▲                ▲
        │                │                │
   ┌────┴────┐    ┌─────┴─────┐    ┌─────┴─────┐
   │  REST   │    │ WebSocket │    │  RabbitMQ │
   │ Adapter │    │  Adapter  │    │  Adapter  │
   └─────────┘    └───────────┘    └───────────┘
```

**Patterns to implement:**
- **Adapter Pattern** – each provider behind common interface
- **Normalizer** – different formats → one domain model
- **Content Enricher** – enriching data from multiple sources
- **Aggregator** – combining data from multiple providers
- **Provider Fallback** – when main provider fails, use backup
- **Provider Health** – monitoring health of each provider

**Testing:**
- Contract tests for each adapter
- Integration tests with mocks
- Failover tests

**At work:**
- Identify which patterns you already use
- Propose refactor to cleaner architecture

**Verification:**
- [ ] I have common interface for all providers
- [ ] Adding new provider doesn't require domain changes
- [ ] I have fallback between providers
- [ ] Each adapter is testable in isolation
- [ ] I can explain architecture to new team member

---

## SPRINT 6.5: Domain Mapping & Conflict Resolution ⭐

**Duration:** 6 weeks

**Goal:** I solve logical problems when combining multiple data sources – identifier mapping, conflicts between providers, establishing "source of truth".

**Resource:**
- "Enterprise Integration Patterns" – Aggregator, Content-Based Router
- Articles on Master Data Management
- Industry case studies (how exchanges resolve data conflicts)

**Project:**
Conflict resolution system for multi-provider setup:

**Domain Mapping:**
```
Problem: Provider A calls market "1X2"
         Provider B calls same market "Match Winner"
         Provider C uses "FT_1X2"

Solution: Mapping table + your internal standard
```
- Mapping table: external_id → internal_id
- Handling new/unknown identifiers
- Fuzzy matching for names (e.g., "Real Madrid" vs "Real Madrid CF")
- Mapping versioning (when provider changes naming)

**Master Provider Logic:**
```
Problem: Provider A says: match LIVE
         Provider B says: match FINISHED
         Who is right?

Solution: Provider hierarchy + business rules
```
- Provider priority configuration per event type
- Rules: "if main provider sends FINISHED, ignore LIVE from others"
- Override rules (e.g., "for this league Provider B is better")
- Audit log: "used data from Provider A, ignored Provider B"

**Conflict Resolution Strategies:**
- **Latest wins** – take newest timestamp
- **Priority wins** – take from provider with higher priority
- **Best value** – take better odds (for odds)
- **Consensus** – take value when >= 2 providers agree
- **Manual review** – flag for manual verification

**State Management:**
```
Event State Machine:
PREMATCH → LIVE → HALFTIME → LIVE → FINISHED → SETTLED

Rules:
- Don't revert state (FINISHED → LIVE = error or delayed provider)
- Log anomalies
```

**At work:**
- Map how you currently resolve conflicts
- Propose improvements

**Verification:**
- [ ] I have mapping table for identifiers
- [ ] I understand how to handle unknown identifier from provider
- [ ] I have defined provider hierarchy
- [ ] I can explain what happens when providers disagree
- [ ] I have audit log of "which provider won" decisions
- [ ] I understand state machine for events and don't revert states

---

## SPRINT 6.6: Advanced EIP – Resequencer, Splitter, Wire Tap ⭐

**Duration:** 6 weeks

**Goal:** I know advanced integration patterns needed to build reliable real-time system.

**Resource:**
- "Enterprise Integration Patterns" (Gregor Hohpe) – chapters 6-10
- Articles on event ordering in distributed systems

**Project:**
Implementation of advanced patterns:

**Resequencer (CRITICAL for real-time):**
```
Problem: Packets arrive out of order
         Packet 3: "Goal scored"
         Packet 2: "Foul committed"
         Packet 1: "Match started"

Solution: Buffer + sorting by sequence number/timestamp
```
- Buffer for out-of-order messages
- Timeout: "wait max 500ms for missing packets"
- Gap detection: "missing packet #5, have #4 and #6"
- Release strategy: when to release from buffer

**Splitter:**
```
Problem: Provider sends one batch with 50 matches

Solution: Split into 50 separate messages
```
- Splitting batch messages into individual
- Preserving context (correlation ID)
- Parallel processing after split
- Error handling: what when 1 of 50 fails

**Wire Tap (debugging):**
```
Problem: "Why did system show wrong match result?"

Solution: Copy ALL raw messages to archive
```
- Copying raw messages without affecting main flow
- Archive for debugging
- Retention policy (how long to keep)
- Search: "show all messages for match X from day Y"

**Content Enricher:**
```
Problem: Provider sends only team_id: 123

Solution: Pull name, logo, colors from database
```
- Cache for enrichment data
- Fallback when enrichment fails
- Async vs sync enrichment

**Process Manager:**
```
Problem: Sequence of actions dependent on state
         "If Provider A suspends market, check B,
          if B also suspends, send alert"

Solution: State machine for complex processes
```
- Defining processes as state machines
- State persistence (what if restart mid-process)
- Timeout handling
- Compensation actions

**Idempotency:**
```
Problem: Same event can arrive 2x (retry, duplicate from provider)

Solution: Deduplication + idempotent processing
```
- Message deduplication (by ID + timestamp)
- Idempotent handlers (processing 2x = same effect)
- Deduplication window (how long to remember)

**At work:**
- Identify where resequencer is missing
- Add Wire Tap for debugging

**Verification:**
- [ ] I have resequencer for out-of-order messages
- [ ] I can split batch into individual messages (splitter)
- [ ] I have Wire Tap for archiving raw messages
- [ ] I can enrich message with additional data (enricher)
- [ ] I understand idempotency and have deduplication
- [ ] I can diagnose problem using archived messages

---

## SPRINT 7: Integration Testing & Resilience Testing ⭐

**Duration:** 4 weeks

**Goal:** I can test integrations in a way that gives confidence they work in production.

**Resource:**
- WireMock documentation
- Testcontainers documentation (RabbitMQ)
- Articles on chaos engineering basics

**Project:**
Test suite for integration layer:

**Contract Testing (WireMock):**
- Mock each external API
- Happy path tests
- Error scenario tests (4xx, 5xx, timeout)
- Edge case tests (empty response, malformed JSON)

**Integration Testing (Testcontainers):**
- RabbitMQ in container
- Full flow tests
- Connection failure and recovery tests

**Resilience Testing:**
- Test retry behavior
- Test circuit breaker opening/closing
- Test fallback activation
- Test graceful degradation

**Chaos Testing (basics):**
- Simulate network latency
- Simulate connection drop
- Simulate partial failure

**At work:**
- Add WireMock tests to your integration

**Verification:**
- [ ] I have tests for happy path and error scenarios
- [ ] Tests don't require real external API
- [ ] I test retry and circuit breaker
- [ ] I can simulate failure in test

---

## SPRINT 7.5: Reconciliation & Graceful Degradation ⭐

**Duration:** 4 weeks

**Goal:** My system knows when data is stale and can operate in "degraded" mode without crashing.

**Resource:**
- "Release It!" – chapters on stability patterns
- Articles on graceful degradation
- Case studies from large systems (Netflix, Amazon)

**Project:**

**Reconciliation (State synchronization):**
```
Problem: Your local state can drift from provider state
         (missed messages, bugs, network issues)

Solution: Periodic comparison and repair
```
- Full sync endpoint: "give me everything again"
- Delta sync: "give me changes since timestamp X"
- Scheduled reconciliation (e.g., every hour)
- On-demand reconciliation ("sync now" button)
- Reconciliation report: "found 5 differences, fixed 3"

**Graceful Degradation:**
```
Problem: Provider is unavailable
         What do you show the user?

Solution: Fallback hierarchy
```
- **Level 1:** Use backup provider
- **Level 2:** Use cached data (marked as "stale")
- **Level 3:** Hide section/show "temporarily unavailable"
- **Level 4:** Show static fallback

**Data Freshness Indicators:**
```java
class EventData {
    Event event;
    Instant lastUpdated;
    String sourceProvider;
    FreshnessStatus status; // LIVE, STALE, UNKNOWN
}
```
- Marking data age
- UI indicator: "last updated 30 seconds ago"
- Alert on stale data (for operators)

**Self-Healing:**
- Automatic switching to backup
- Automatic retry after time
- Automatic restoration of main provider when it returns

**At work:**
- Identify what happens when provider is unavailable
- Propose degradation strategy

**Verification:**
- [ ] I have reconciliation job that compares state
- [ ] System works (degraded) when main provider is unavailable
- [ ] User knows data might be stale (UI indicator)
- [ ] System automatically returns to normal state
- [ ] I have alerts for stale data and degraded mode

---

## SPRINT 8: SQL – Performance fundamentals

**Duration:** 4 weeks

**Goal:** I understand how database executes queries. I can read EXPLAIN and see where query is slow.

**Resource:**
- [Use The Index, Luke!](https://use-the-index-luke.com/){:target="_blank"} (online, free) – chapters 1-4
- EXPLAIN documentation for your database (PostgreSQL/MySQL)

**Project:**
Add database layer to "Mini Feed Service":
- Tables: events, markets, outcomes
- Write 5 different queries (simple, with JOIN, with aggregation)
- For each query: run EXPLAIN ANALYZE
- Add indexes and compare execution times
- Document: before vs after

**At work:**
- Find slowest query in your service
- Run EXPLAIN ANALYZE
- Propose index or refactor

**Verification:**
- [ ] I can read EXPLAIN and point where the problem is
- [ ] I understand difference between Seq Scan and Index Scan
- [ ] I know what covering index is and when to use it
- [ ] I understand N+1 problem and how to avoid it

---

## SPRINT 9: SQL – Transactions and isolation

**Duration:** 4 weeks

**Goal:** I understand what happens when two processes modify the same data.

**Resource:**
- "Designing Data-Intensive Applications" – chapter 7 (Transactions)
- Isolation levels documentation for your database

**Project:**
Write tests demonstrating concurrency problems:
- Dirty read
- Non-repeatable read
- Phantom read
- Lost update

For each:
- Code showing the problem
- Code showing the solution

**At work:**
- Check what isolation level you use by default

**Verification:**
- [ ] I can explain difference between READ COMMITTED and SERIALIZABLE
- [ ] I know when to use SELECT FOR UPDATE
- [ ] I understand difference between optimistic and pessimistic locking

---

## SPRINT 10: Redis – Basics

**Duration:** 4 weeks

**Goal:** I know basic Redis data structures and when to use which.

**Resource:**
- Redis University – "RU101: Introduction to Redis Data Structures" (free)
- Redis documentation

**Project:**
Add Redis to "Mini Feed Service":
- Cache for GET /events/{id}
- Key design: `event:{id}`, `event:{id}:markets`
- TTL for different data types
- Health check

**At work:**
- Analyze key structure in project

**Verification:**
- [ ] I know difference between STRING, HASH, LIST, SET, SORTED SET
- [ ] I can design key structure
- [ ] I understand TTL

---

## SPRINT 11: Redis – Cache patterns

**Duration:** 4 weeks

**Goal:** I understand different caching strategies.

**Resource:**
- "Designing Data-Intensive Applications" – cache sections
- Articles on cache patterns

**Project:**
Extend cache in "Mini Feed Service":
- Cache-aside pattern
- Cache invalidation on update
- Metrics: cache hits, misses

Document comparing cache-aside vs read-through vs write-through.

**At work:**
- Measure hit ratio of your cache

**Verification:**
- [ ] I can explain different cache strategies
- [ ] I know how to handle cache invalidation
- [ ] I understand "thundering herd" problem

---

## SPRINT 12: TDD – Test-Driven Development

**Duration:** 6 weeks

**Goal:** I write tests before code. TDD is my default way of working.

**Resource:**
- "Test-Driven Development by Example" (Kent Beck)
- "Growing Object-Oriented Software, Guided by Tests" (GOOS) – chapters 1-10

**Project:**
Write new module using TDD:
- Feed event validator (validating data from provider)
- Red → Green → Refactor for each functionality
- Document process (git commits showing TDD cycle)

Rules:
- Don't write production code without failing test
- Write minimal code to make test pass
- Refactor after each green test

**At work:**
- Use TDD for next task

**Verification:**
- [ ] I can work in Red → Green → Refactor cycle
- [ ] My tests are fast (<100ms each)
- [ ] Tests document behavior, not implementation
- [ ] I feel comfortable writing test before code

---

## SPRINT 13: Unit tests – Best practices

**Duration:** 4 weeks

**Goal:** My tests are readable, fast and maintainable.

**Resource:**
- "Unit Testing Principles, Practices, and Patterns" (Vladimir Khorikov)

**Project:**
Review and refactor existing tests:
- AAA structure (Arrange-Act-Assert)
- Names describing behavior
- Parameterized tests
- Remove duplication (but not at cost of readability)

**At work:**
- Review 5 random tests in project

**Verification:**
- [ ] My tests are readable without looking at production code
- [ ] I know what test double is and when to use mock vs stub vs fake
- [ ] I don't test implementation, only behavior

---

## SPRINT 14: Integration tests – Testcontainers

**Duration:** 4 weeks

**Goal:** I test with real database and Redis without manual setup.

**Resource:**
- Testcontainers documentation
- Testcontainers + Spring Boot guides

**Project:**
Integration tests for "Mini Feed Service":
- PostgreSQL in container
- Redis in container
- Full flow tests

**At work:**
- Add integration test to existing service

**Verification:**
- [ ] Tests work locally and in CI
- [ ] I know how to isolate tests

---

## SPRINT 15: Logging & Observability

**Duration:** 4 weeks

**Goal:** My logs are searchable and contain needed context.

**Resource:**
- Logback/Log4j2 documentation
- Articles on structured logging

**Project:**
Structured logging in "Mini Feed Service":
- JSON format
- Consistent fields: timestamp, level, service, traceId
- MDC for request context

**At work:**
- Review logs from last incident

**Verification:**
- [ ] My logs are in JSON with consistent fields
- [ ] I can trace request across multiple services
- [ ] I don't log sensitive data

---

## SPRINT 16: Metrics – Micrometer & Prometheus

**Duration:** 4 weeks

**Goal:** My service exposes metrics showing its health.

**Resource:**
- Micrometer documentation
- Prometheus documentation

**Project:**
Metrics for "Mini Feed Service":
- Request rate, error rate, latency (RED)
- Cache hit/miss ratio
- Connection pool metrics
- Custom: events processed/second
- Simple Grafana dashboard

**At work:**
- Check what metrics your service exposes

**Verification:**
- [ ] I understand counter vs gauge vs histogram
- [ ] I know why p99 > average
- [ ] I can create dashboard

---

## SPRINT 17: Kafka – Basics and production patterns

**Duration:** 6 weeks

**Goal:** I understand Kafka and can write reliable producer/consumer with event ordering preservation.

**Resource:**
- "Kafka: The Definitive Guide" – chapters 1-6
- Confluent Kafka 101 (free course)
- Articles on event ordering in Kafka

**Project:**
Kafka in "Mini Feed Service":

**Basics:**
- Producer publishing events
- Consumer with manual ack
- Consumer group
- DLQ for failed messages
- Schema Registry + Avro (optional)

**Event Ordering (CRITICAL):**
```
Problem: "Match Started" must be BEFORE "Goal Scored"
         But Kafka guarantees order only in ONE partition

Solution: Partitioning strategy
```

**Partitioning Strategy:**
```java
// Option 1: Partition by event_id (all events for one match in one partition)
producer.send(new ProducerRecord<>("feed", eventId, message));

// Option 2: Partition by provider (preserve order per provider)
producer.send(new ProducerRecord<>("feed", providerId, message));

// Option 3: Custom partitioner (e.g., by sport + league)
```
- Trade-off: more partitions = better parallelism, but harder ordering
- Single partition per event = guaranteed order, but bottleneck
- Document decision in ADR

**Exactly-once vs At-least-once:**
- When "at-least-once" is enough (idempotent consumer)
- When you need "exactly-once" (Kafka transactions)
- Cost of exactly-once (performance)

**Late Events:**
```
Problem: Event arrives with 5 minute delay
         (e.g., provider had problem and sent from backlog)

Solution:
- Watermarks (event time vs processing time)
- Late event handling policy
- Reprocessing strategy
```

**At work:**
- Analyze consumer lag
- Check partitioning strategy
- Is event ordering preserved?

**Verification:**
- [ ] I understand partition, offset, consumer group
- [ ] I know how to configure retry and DLQ
- [ ] I can diagnose consumer lag
- [ ] I understand how partitioning affects ordering
- [ ] I can choose partitioning strategy for my use case
- [ ] I know how to handle late events

---

# PHASE 2: Java Concurrency & JVM

*Sprints 18-29 | Goal: Deep understanding of JVM and concurrency*

---

## SPRINT 18: Java Memory Model – Basics

**Duration:** 6 weeks

**Goal:** I understand why concurrent code can behave "weirdly".

**Resource:**
- "Java Concurrency in Practice" – chapters 1-3

**Project:**
Examples demonstrating:
- Visibility problem (without volatile)
- Reordering
- Safe publication
- Immutable objects

**Verification:**
- [ ] I understand visibility and volatile
- [ ] I know what happens-before is
- [ ] I can identify race condition

---

## SPRINT 19: Synchronization – Locks

**Duration:** 4 weeks

**Goal:** I know how to use synchronization and what its costs are.

**Resource:**
- "Java Concurrency in Practice" – chapters 4-5

**Project:**
Thread-safe cache with different strategies:
- synchronized
- ReentrantLock
- ReadWriteLock
- Benchmark each version

**Verification:**
- [ ] I know when to use synchronized vs Lock
- [ ] I understand ReadWriteLock
- [ ] I can identify contention point

---

## SPRINT 20: Concurrent Collections

**Duration:** 4 weeks

**Goal:** I know thread-safe collections from java.util.concurrent.

**Resource:**
- "Java Concurrency in Practice" – chapter 5

**Project:**
Implementation comparison:
- ConcurrentHashMap vs synchronized map
- BlockingQueue implementations
- CopyOnWriteArrayList

**Verification:**
- [ ] I know when to use which collection
- [ ] I understand CAS in ConcurrentHashMap

---

## SPRINT 21: ExecutorService & Thread Pools

**Duration:** 4 weeks

**Goal:** I can configure thread pool for specific use case.

**Resource:**
- "Java Concurrency in Practice" – chapters 6-8

**Project:**
Asynchronous processing in "Mini Feed Service":
- ThreadPoolExecutor with custom configuration
- Rejection handling
- Graceful shutdown
- Metrics

**Verification:**
- [ ] I can configure ThreadPoolExecutor
- [ ] I know how to size the pool
- [ ] I understand rejection strategies

---

## SPRINT 22: CompletableFuture

**Duration:** 4 weeks

**Goal:** I can compose asynchronous operations.

**Resource:**
- CompletableFuture documentation
- "Modern Java in Action"

**Project:**
Asynchronous flow:
- Fetch data from multiple sources in parallel
- Combine results
- Handle errors and timeouts

**Verification:**
- [ ] I understand thenApply vs thenCompose
- [ ] I can handle errors in async flow
- [ ] I know how to set timeout

---

## SPRINT 23: Virtual Threads (Project Loom)

**Duration:** 4 weeks

**Goal:** I understand Virtual Threads and know when to use them.

**Resource:**
- JEP 444: Virtual Threads
- Articles on Foojay.io

**Project:**
Comparison of Virtual Threads with platform threads:
- Benchmark for I/O-bound workload
- Migration of existing code

**Verification:**
- [ ] I understand difference between platform and virtual threads
- [ ] I know limitations (pinning)
- [ ] I know when they give advantage

---

## SPRINT 24: GC – Basics and G1

**Duration:** 6 weeks

**Goal:** I understand Garbage Collector and can read GC logs.

**Resource:**
- "Java Performance" (Scott Oaks) – GC chapters

**Project:**
GC analysis:
- Enable GC logging
- Analyze logs (GCEasy)
- Experiment with heap size

**Verification:**
- [ ] I understand G1 phases
- [ ] I can read GC logs
- [ ] I know what allocation rate means

---

## SPRINT 25: Profiling – JFR & async-profiler

**Duration:** 4 weeks

**Goal:** I can profile application and find bottlenecks.

**Resource:**
- JFR and async-profiler documentation

**Project:**
Profile "Mini Feed Service":
- CPU profiling
- Allocation profiling
- Lock profiling
- Flame graphs

Optimize one found spot.

**Verification:**
- [ ] I can run profiler
- [ ] I can read flame graphs
- [ ] I can identify hotspot

---

## SPRINT 26: JVM Tuning

**Duration:** 4 weeks

**Goal:** I can tune JVM.

**Resource:**
- "Java Performance" – tuning chapters

**Project:**
JVM configuration experiments:
- Heap sizing
- GC selection (G1 vs ZGC)
- Benchmark different configurations

**Verification:**
- [ ] I know how to size heap
- [ ] I understand difference G1 vs ZGC
- [ ] I know when not to tune

---

## SPRINT 27: Spring Boot – Under the hood

**Duration:** 6 weeks

**Goal:** I understand how Spring Boot works.

**Resource:**
- Spring documentation
- Source code (selected classes)

**Project:**
Deep dive:
- Auto-configuration
- Bean lifecycle
- @Transactional (proxy)
- Connection pooling

**Verification:**
- [ ] I understand auto-configuration
- [ ] I know how Spring proxy works
- [ ] I understand propagation in @Transactional

---

## SPRINT 28: Lock-free – Atomics and CAS

**Duration:** 6 weeks

**Goal:** I understand lock-free algorithms.

**Resource:**
- "Java Concurrency in Practice" – chapter 15

**Project:**
Lock-free implementations:
- Counter (AtomicLong)
- Stack (CAS)
- Benchmark vs locks

**Verification:**
- [ ] I understand CAS
- [ ] I know when lock-free wins
- [ ] I understand ABA problem

---

## SPRINT 29: Reactive – Basics (optional)

**Duration:** 6 weeks

**Goal:** I understand reactive programming.

**Resource:**
- Project Reactor documentation

**Project:**
Part of service on reactive:
- WebFlux
- R2DBC
- Comparison with imperative

**Verification:**
- [ ] I understand backpressure
- [ ] I can debug reactive code
- [ ] I know when it makes sense

---

# PHASE 3: Go + Algorithms

*Sprints 30-40 | Goal: Go as second tool + practical algorithms*

---

## SPRINT 30: Go – Language basics

**Duration:** 6 weeks

**Goal:** I know Go syntax and can write simple program.

**Resource:**
- "The Go Programming Language" – chapters 1-4
- A Tour of Go

**Project:**
CLI tool: fetches events from API, displays in table.

**Verification:**
- [ ] I know basic syntax
- [ ] I understand slice vs array
- [ ] I can define structs

---

## SPRINT 31: Go – Error handling and idioms

**Duration:** 4 weeks

**Goal:** I write idiomatic Go code.

**Resource:**
- "The Go Programming Language" – chapters 5, 7
- Effective Go

**Project:**
CLI extension:
- Custom error types
- Error wrapping
- Documentation (godoc)

**Verification:**
- [ ] I understand "errors are values"
- [ ] My code passes go vet and golint

---

## SPRINT 32: Algorithms in Go – Hash Tables

**Duration:** 4 weeks

**Goal:** I understand hash tables and implement my own in Go.

**Resource:**
- "Introduction to Algorithms" (CLRS) – hash tables chapter
- Or: "Grokking Algorithms" – simpler introduction

**Project:**
Implementation in Go:
- Simple hash table (open addressing or chaining)
- Collision handling
- Resize when full
- Benchmark vs built-in map

Use case: odds cache (key: eventId+marketId, value: odds)

**Verification:**
- [ ] I understand how hashing works
- [ ] I know how to handle collisions
- [ ] I understand complexity O(1) average, O(n) worst

---

## SPRINT 33: Algorithms in Go – Priority Queues & Heaps

**Duration:** 4 weeks

**Goal:** I understand heap and priority queue.

**Resource:**
- CLRS – heaps chapter
- Go heap package documentation

**Project:**
Implementation in Go:
- Min-heap from scratch
- Priority queue using container/heap
- Benchmark

Use case: processing events by priority (e.g., live events > prematch)

**Verification:**
- [ ] I understand heap structure
- [ ] I know when to use priority queue
- [ ] I can use container/heap

---

## SPRINT 34: Algorithms in Go – LRU Cache

**Duration:** 4 weeks

**Goal:** I implement LRU cache from scratch.

**Resource:**
- LeetCode problem #146 (LRU Cache)
- Articles on LRU implementation

**Project:**
LRU Cache in Go:
- Hash map + doubly linked list
- O(1) get and put
- Configurable capacity
- Eviction callback (optional)

Use case: cache of recently used events/markets in memory

**Verification:**
- [ ] I understand why hash map + linked list is needed
- [ ] My implementation is O(1)
- [ ] I can explain trade-offs vs other strategies (LFU, FIFO)

---

## SPRINT 35: Go – Goroutines and channels

**Duration:** 6 weeks

**Goal:** I understand concurrency in Go.

**Resource:**
- "The Go Programming Language" – chapters 8-9
- "Concurrency in Go" (Katherine Cox-Buday)

**Project:**
Feed processor:
- Worker pool
- Fan-out / Fan-in
- Pipeline pattern

**Verification:**
- [ ] I understand buffered vs unbuffered channel
- [ ] I can use select
- [ ] I can implement worker pool

---

## SPRINT 36: Go – Context and cancellation

**Duration:** 4 weeks

**Goal:** I use context correctly.

**Resource:**
- context package documentation
- "Go Concurrency Patterns: Context"

**Project:**
Feed processor extension:
- Context with timeout
- Context propagation
- Graceful cancellation

**Verification:**
- [ ] I understand context.Background vs TODO
- [ ] I can set timeout

---

## SPRINT 37: Go – HTTP and services

**Duration:** 6 weeks

**Goal:** I can write production HTTP service in Go.

**Resource:**
- "Go Systems Programming"
- net/http documentation

**Project:**
"Feed Ingestor" in Go:
- HTTP server
- Health check
- Metrics (Prometheus)
- Graceful shutdown

**Verification:**
- [ ] I can write HTTP server in Go
- [ ] I understand middleware pattern
- [ ] I can do graceful shutdown

---

## SPRINT 38: Go – Testing

**Duration:** 4 weeks

**Goal:** I write good tests in Go.

**Resource:**
- testing package documentation

**Project:**
Tests for "Feed Ingestor":
- Table-driven tests
- Testcontainers-go
- Benchmark tests

**Verification:**
- [ ] I write table-driven tests
- [ ] I can benchmark

---

## SPRINT 39: Go – Production readiness

**Duration:** 4 weeks

**Goal:** My Go service is production ready.

**Resource:**
- pprof documentation

**Project:**
"Feed Ingestor" polish:
- pprof endpoint
- Configuration (env vars)
- Docker image
- Documentation

**Verification:**
- [ ] Service works in container
- [ ] I can profile it (pprof)

---

## SPRINT 40: Go – CLI tools

**Duration:** 4 weeks

**Goal:** I write useful CLI tools.

**Resource:**
- Cobra library

**Project:**
CLI tools:
1. `feed-replay` – replays events from file
2. `feed-stats` – feed statistics
3. `feed-diff` – provider comparison

**Verification:**
- [ ] My tools have sensible --help
- [ ] They are useful at work

---

# PHASE 4: Architecture and Patterns

*Sprints 41-52 | Goal: Designing distributed systems*

---

## SPRINT 41: Enterprise Integration Patterns – Deeper

**Duration:** 6 weeks

**Goal:** I know advanced integration patterns.

**Resource:**
- "Enterprise Integration Patterns" – chapters 6-13

**Project:**
Identify and implement:
- Splitter / Aggregator
- Resequencer
- Process Manager
- Wire Tap (for monitoring)

**Verification:**
- [ ] I can recognize pattern in existing code
- [ ] I know when to use which

---

## SPRINT 42: API Design – gRPC

**Duration:** 4 weeks

**Goal:** I know when to use gRPC instead of REST.

**Resource:**
- gRPC documentation
- Protobuf documentation

**Project:**
Inter-service communication via gRPC:
- .proto for feed events
- Server in Java, client in Go
- Streaming

**Verification:**
- [ ] I can define .proto
- [ ] I understand streaming in gRPC
- [ ] I know when gRPC vs REST

---

## SPRINT 43: Event-Driven Architecture

**Duration:** 6 weeks

**Goal:** I design event-driven systems.

**Resource:**
- "Designing Data-Intensive Applications" – streaming
- Articles on EDA

**Project:**
Event-driven fragment of system:
- Event schema
- Event catalog
- Producers/consumers
- Event flow diagrams

**Verification:**
- [ ] I understand events vs commands vs queries
- [ ] I can design event schema
- [ ] I understand eventual consistency

---

## SPRINT 44: CQRS

**Duration:** 4 weeks

**Goal:** I understand CQRS and know when it makes sense.

**Resource:**
- Martin Fowler – CQRS
- Greg Young – presentations

**Project:**
CQRS for part of system:
- Command side
- Query side (read models)
- Synchronization

**Verification:**
- [ ] I understand command/query separation
- [ ] I know when NOT to use CQRS

---

## SPRINT 45: Event Sourcing

**Duration:** 6 weeks

**Goal:** I understand Event Sourcing for trading systems.

**Resource:**
- Greg Young – Event Sourcing
- "Implementing DDD"

**Project:**
Event-sourced aggregate:
- Events: MarketCreated, OddsUpdated, etc.
- Rebuild state from events
- Snapshots
- Projections

**Verification:**
- [ ] I understand state vs event sourcing
- [ ] I can do replay

---

## SPRINT 46: Resiliency Patterns

**Duration:** 6 weeks

**Goal:** I design systems resistant to failures.

**Resource:**
- "Release It!" (Michael Nygard)
- Resilience4j documentation

**Project:**
Resilience in "Feed Ingestor":
- Circuit Breaker
- Retry
- Bulkhead
- Rate Limiter
- Chaos testing

**Verification:**
- [ ] I understand Circuit Breaker
- [ ] I can configure retry with backoff
- [ ] I can test resilience

---

## SPRINT 47: Distributed Systems – Consistency

**Duration:** 6 weeks

**Goal:** I understand consistency models.

**Resource:**
- "Designing Data-Intensive Applications" – chapters 5, 7, 9

**Project:**
Consistency analysis in your system:
- Where strong, where eventual
- What happens during partition
- Document with trade-offs

**Verification:**
- [ ] I understand CAP theorem
- [ ] I can explain consistency models
- [ ] I know where strong is required

---

## SPRINT 48: Distributed Systems – Failure modes

**Duration:** 4 weeks

**Goal:** I understand how distributed systems fail.

**Resource:**
- "Designing Data-Intensive Applications" – chapter 8
- Google SRE Book

**Project:**
Failure mode analysis for feed pipeline:
- What on failure of each component
- How to detect, fix, prevent

**Verification:**
- [ ] I can predict behavior on failure
- [ ] I know how to design for failure

---

## SPRINT 49: Multi-region Architecture

**Duration:** 6 weeks

**Goal:** I design multi-region systems.

**Resource:**
- AWS/GCP multi-region guides
- DDIA – replication

**Project:**
Multi-region architecture for feed platform:
- Active-active vs active-passive
- Data replication
- Failover mechanism

**Verification:**
- [ ] I understand active-active vs passive
- [ ] I know how to replicate data
- [ ] I can design failover

---

## SPRINT 50: Multi-tenant Architecture

**Duration:** 4 weeks

**Goal:** I design multi-tenant systems.

**Resource:**
- Multi-tenant architecture articles

**Project:**
Multi-tenant for betting platform:
- Tenant isolation
- Shared vs dedicated resources
- Configuration per tenant

**Verification:**
- [ ] I understand model trade-offs
- [ ] I know how to isolate tenants

---

## SPRINT 51: System Design Practice

**Duration:** 6 weeks

**Goal:** I can design systems on whiteboard.

**Resource:**
- "System Design Interview" (Alex Xu)

**Project:**
Design 3 systems:
1. Real-time sports feed aggregator
2. Betting exchange
3. Live odds distribution

**Verification:**
- [ ] I can gather requirements
- [ ] I can draw high-level architecture
- [ ] I can discuss trade-offs

---

## SPRINT 52: Kafka Internals

**Duration:** 4 weeks

**Goal:** I understand how Kafka works internally.

**Resource:**
- Kafka documentation – internals
- "Kafka: The Definitive Guide" – advanced chapters

**Project:**
Deep dive:
- Log structure on disk
- Replication protocol
- Consumer coordination
- Compaction

Write document explaining internals for team.

**Verification:**
- [ ] I understand how Kafka persists data
- [ ] I know how replication works
- [ ] I can explain compaction

---

# PHASE 5: Low-Latency

*Sprints 53-64 | Goal: Ultra-efficient trading systems*

---

## SPRINT 53: Hot Path Optimization

**Duration:** 4 weeks

**Goal:** I can identify and optimize hot path.

**Resource:**
- Mechanical Sympathy blog

**Project:**
Hot path analysis in feed handler:
- Identify hot path
- Measure latency of each step
- Separate hot path from slow path
- Optimize

**Verification:**
- [ ] I can identify hot path
- [ ] I understand slow path separation

---

## SPRINT 54: ZGC

**Duration:** 4 weeks

**Goal:** I can tune GC for ultra-low latency.

**Resource:**
- ZGC documentation and JEPs

**Project:**
Migration to ZGC:
- Configuration
- Benchmark vs G1
- Tuning

**Verification:**
- [ ] I understand how ZGC works
- [ ] I can configure ZGC
- [ ] I know when ZGC > G1

---

## SPRINT 55: Off-heap Memory

**Duration:** 6 weeks

**Goal:** I use off-heap memory.

**Resource:**
- ByteBuffer documentation
- Articles on off-heap

**Project:**
Off-heap cache:
- Direct ByteBuffer
- Memory-mapped files
- Comparison with on-heap

**Verification:**
- [ ] I understand heap vs off-heap
- [ ] I can use ByteBuffer
- [ ] I understand trade-offs

---

## SPRINT 56: Chronicle Queue

**Duration:** 6 weeks

**Goal:** I use Chronicle Queue.

**Resource:**
- Chronicle Queue documentation
- Chronicle Software blog

**Project:**
Feed handler with Chronicle Queue:
- Persisted queue
- Zero-GC reading/writing
- Replay

**Verification:**
- [ ] I can use Chronicle Queue
- [ ] I understand how it works (mmap)
- [ ] I can do replay

---

## SPRINT 57: LMAX Disruptor

**Duration:** 6 weeks

**Goal:** I use Disruptor.

**Resource:**
- Disruptor documentation
- Martin Thompson presentations

**Project:**
Feed handler with Disruptor:
- Ring buffer
- Producer -> Consumer pipeline
- Batch processing
- Benchmark

**Verification:**
- [ ] I understand Ring Buffer
- [ ] I can configure Disruptor
- [ ] I understand wait strategies

---

## SPRINT 58: SBE (Simple Binary Encoding)

**Duration:** 4 weeks

**Goal:** I use SBE for ultra-fast serialization.

**Resource:**
- SBE specification
- real-logic/simple-binary-encoding

**Project:**
Feed events with SBE:
- Schema definition
- Code generation
- Zero-copy access
- Benchmark vs JSON, Protobuf

**Verification:**
- [ ] I can define SBE schema
- [ ] I understand zero-copy
- [ ] I know when SBE vs Protobuf

---

## SPRINT 59: Zero-GC Techniques

**Duration:** 6 weeks

**Goal:** I write code without allocations on hot path.

**Resource:**
- Chronicle Software blog
- Martin Thompson presentations

**Project:**
Zero-allocation feed handler:
- Object pooling
- Primitive collections (Agrona)
- Flyweight pattern
- Verification with allocation profiler

**Verification:**
- [ ] I can identify allocation hotspots
- [ ] I know how to use object pooling
- [ ] I can write code without allocations on hot path

---

## SPRINT 60: Network Tuning

**Duration:** 4 weeks

**Goal:** I understand network latency.

**Resource:**
- TCP tuning guides
- "Systems Performance" (Brendan Gregg)

**Project:**
Network optimization:
- TCP_NODELAY
- Socket buffer tuning
- Connection pooling
- Benchmark

**Verification:**
- [ ] I understand Nagle's algorithm
- [ ] I know how to tune socket buffers

---

## SPRINT 61: Aeron

**Duration:** 6 weeks

**Goal:** I know Aeron.

**Resource:**
- Aeron documentation
- Martin Thompson presentations

**Project:**
POC with Aeron:
- Reliable UDP
- Publisher/Subscriber
- Comparison with TCP, Kafka

**Verification:**
- [ ] I understand how Aeron works
- [ ] I know when to use it

---

## SPRINT 62: Determinism & Replay

**Duration:** 6 weeks

**Goal:** I design deterministic systems.

**Resource:**
- Event Sourcing resources
- Trading systems case studies

**Project:**
Deterministic feed handler:
- All inputs as events
- Replay capability
- Test: 2x same input = identical output

**Verification:**
- [ ] I understand why determinism matters
- [ ] I can design deterministic system

---

## SPRINT 63: JMH & Microbenchmarking

**Duration:** 4 weeks

**Goal:** I write reliable microbenchmarks.

**Resource:**
- JMH documentation
- Aleksey Shipilёv articles

**Project:**
Benchmarks:
- Serialization (JSON vs Protobuf vs SBE)
- Collections
- Queues

**Verification:**
- [ ] I can write correct JMH benchmark
- [ ] I understand warmup and dead code elimination
- [ ] I know how to interpret results

---

## SPRINT 64: Full System Optimization

**Duration:** 6 weeks

**Goal:** I optimize entire system end-to-end.

**Project:**
"Ultra Low Latency Feed Handler":
- Disruptor
- Chronicle Queue
- SBE
- ZGC
- Zero-allocation

Target: p99 < 1ms

**Verification:**
- [ ] I built system with p99 < 1ms
- [ ] I can explain every decision

---

# PHASE 6: Leadership

*Sprints 65-73 | Goal: Technical leadership*

---

## SPRINT 65: ADRs & Technical Documentation

**Duration:** 4 weeks

**Goal:** I document technical decisions.

**Project:**
- Write 5 ADRs
- Create template for team
- Introduce process

**Verification:**
- [ ] Team uses ADRs
- [ ] Decisions are easy to find

---

## SPRINT 66: Design Reviews

**Duration:** 4 weeks

**Goal:** I lead effective design reviews.

**Project:**
- Design doc template
- Conduct 3 design reviews
- Gather feedback

**Verification:**
- [ ] I can lead design review
- [ ] I give constructive feedback

---

## SPRINT 67: Mentoring

**Duration:** 4 weeks + ongoing

**Goal:** I develop others.

**Resource:**
- "The Manager's Path" – mentoring chapters

**Project:**
- Take mentee
- Development plan (3 months)
- Regular 1:1s

**Verification:**
- [ ] Mentee makes progress
- [ ] I give actionable feedback

---

## SPRINT 68: SRE & SLO

**Duration:** 6 weeks

**Goal:** I define SLOs.

**Resource:**
- Google SRE Book

**Project:**
SLO for feed platform:
- SLI
- SLO
- Error budget
- Alerting
- Dashboard

**Verification:**
- [ ] I have defined SLOs
- [ ] We have SLO-based alerting
- [ ] Team understands error budgets

---

## SPRINT 69: RFC Process

**Duration:** 4 weeks

**Goal:** I introduce changes through RFC.

**Project:**
- RFC template
- Write RFC for change
- Guide through review

**Verification:**
- [ ] We have working RFC process
- [ ] Large changes go through RFC

---

## SPRINT 70: Technical Strategy

**Duration:** 6 weeks

**Goal:** I define technical roadmap.

**Resource:**
- "Staff Engineer" (Will Larson)

**Project:**
Technical roadmap (12 months):
- Current state
- Target state
- Gap analysis
- Initiatives

**Verification:**
- [ ] I have clear vision of target state
- [ ] Stakeholder buy-in

---

## SPRINT 71: Cross-team Collaboration

**Duration:** 4 weeks

**Goal:** I collaborate effectively with other teams.

**Project:**
- Map dependencies
- Define contracts
- Establish communication channels

**Verification:**
- [ ] I have clear contracts
- [ ] Communication is effective

---

## SPRINT 72: Incident Management

**Duration:** 4 weeks

**Goal:** I lead incidents and learn from them.

**Resource:**
- Google SRE Book – incident management
- PagerDuty guide

**Project:**
- Incident response process
- Runbooks
- Postmortem template
- Conduct postmortem

**Verification:**
- [ ] We have working incident process
- [ ] Postmortems are blameless
- [ ] Team learns from incidents

---

## SPRINT 73: External Presence (optional)

**Duration:** Ongoing

**Goal:** I build expert reputation.

**Project:**
Choose 1-2:
- Blog (1 post/month)
- Meetup presentation
- Open source contribution

**Verification:**
- [ ] I have published content
- [ ] I'm building network

---

# Summary

## Total number of sprints: 76

## Phase breakdown

| Phase | Sprints | Main topic |
|-------|---------|------------|
| 1. Fundamentals + Integrations | 1-17 (+ 6.5, 6.6, 7.5) | HTTP, REST consumer, WebSocket, RabbitMQ, Domain Mapping, Advanced EIP, Reconciliation, SQL, Redis, Kafka, Tests |
| 2. Java Concurrency & JVM | 18-29 | Memory model, locks, pools, GC, profiling |
| 3. Go + Algorithms | 30-40 | Go basics, concurrency, hash tables, heaps, LRU |
| 4. Architecture | 41-52 | EIP, gRPC, EDA, CQRS, ES, resilience, distributed systems |
| 5. Low-Latency | 53-64 | ZGC, off-heap, Chronicle, Disruptor, SBE, zero-GC |
| 6. Leadership | 65-73 | ADR, design reviews, SLO, RFC, strategy |

## Estimated time

| Assumption | Time |
|------------|------|
| Average 6 weeks/sprint | ~9 years |
| Average 8 weeks/sprint (with life) | ~12 years |

## Sprints marked ⭐ (integration priority)

- **Sprint 2:** REST API Consumer – **start here if you have a project**
- **Sprint 3:** REST API Consumer – advanced
- **Sprint 4:** WebSocket + Stale Data Detection
- **Sprint 5:** RabbitMQ
- **Sprint 6:** Integration Patterns (Adapter, Normalizer, Aggregator)
- **Sprint 6.5:** Domain Mapping & Conflict Resolution (Master Provider, ID mapping)
- **Sprint 6.6:** Advanced EIP (Resequencer, Splitter, Wire Tap, Idempotency)
- **Sprint 7:** Integration Testing
- **Sprint 7.5:** Reconciliation & Graceful Degradation
- **Sprint 17:** Kafka + Event Ordering

## Key patterns for integrations (quick reference)

| Pattern | Problem | Sprint |
|---------|---------|--------|
| **Adapter** | Different provider APIs | 6 |
| **Normalizer** | Different market naming | 6 |
| **Resequencer** | Wrong event order | 6.6 |
| **Aggregator** | Data from multiple sources | 6 |
| **Wire Tap** | Debugging disputed results | 6.6 |
| **Master Provider** | Conflict between providers | 6.5 |
| **Stale Data Detection** | "Frozen" feed | 4 |
| **Reconciliation** | Out-of-sync data | 7.5 |
| **Graceful Degradation** | Provider unavailable | 7.5 |

---

*Good luck! Start with Sprint 2 if you have an urgent REST project.*
