---
layout: post
title: "Sprint 2: REST API Consumer"
date: 2025-12-17
categories: [Roadmap, Java]
tags: [rest-api, http-client, resilience4j, spring, learning]
excerpt: "First sprint from the Java Senior → Staff Engineer roadmap. Goal: build a solid REST API client with error handling, retry, and monitoring."
---

Starting work on the [Java Senior → Staff Engineer roadmap](/roadmaps/java-senior/). I'm choosing Sprint 2 because I currently work on external API integrations and want to do it properly from the ground up.

## Sprint Goal

Build a REST API client that:
- properly handles errors (4xx, 5xx)
- implements retry with exponential backoff and jitter
- uses circuit breaker and bulkhead
- exposes metrics (including percentiles) and allows external configuration
- isolates external model from domain

End result: a client resilient to external API failures, production-ready.

## Duration

6 weeks

## Work Plan

### Layer 1 – Basics

**HTTP Client:**
- WebClient or OkHttp
- Timeout configuration (separate connect and read)
- Request and response logging
- Contextual logging with MDC (providerId in context)

**Mapping and domain isolation:**
- Dedicated DTOs for external API (e.g., `ExternalMatchDto`)
- Mapper to domain model (`Event`)
- Business logic doesn't depend on provider's JSON structure

**HTTP error handling:**
- Distinguish 4xx (client error) vs 5xx (server error)
- Different reaction strategies for different codes

**Testing with WireMock:**
- Happy path
- Malformed JSON (null instead of number, string instead of int)
- Empty list, missing fields
- Timeout, 5xx, connection refused

### Layer 2 – Resilience

**Retry:**
- Exponential backoff
- Jitter (random delay) – protection against thundering herd
- Retry only for server errors (5xx), not client errors (4xx)

**Circuit Breaker (Resilience4j):**
- Threshold configuration (failure rate, slow call rate)
- States: CLOSED → OPEN → HALF_OPEN

**Bulkhead:**
- Limit concurrent calls to provider
- One slow integration doesn't block resources for others

**Fallback:**
- Cache with last known value
- Default response
- Mark data as "stale"

**Rate Limiting:**
- Respect provider's API limits

### Layer 3 – Production

**Health Check:**
- Endpoint checking provider availability
- Not just UP/DOWN – also DEGRADED state

**Metrics:**
- Request count, error rate
- Latency: average, P95, P99
- Circuit breaker state
- `provider_last_response_timestamp` – detecting "frozen" feed

**External configuration:**
- URL, timeouts, retry policy
- Circuit breaker thresholds
- Change without recompilation

**Observability:**
- Correlation ID between requests
- Structured logging (JSON)

## Practical Project

Client for external API. Options considered:
- Public sports API (matches, results)
- Mock API with WireMock for testing error scenarios and edge cases

## Resources

### Stability Fundamentals

- **"Release It!" – Michael Nygard** – chapters on Stability Patterns. Focus on: Circuit Breaker (cutting off failures), Bulkhead (resource isolation), Timeouts (response time limits).
- [AWS Builder's Library – Timeouts, retries and backoff with jitter](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/){:target="_blank"} – how to intelligently retry requests and avoid overwhelming the provider during issues.

### Technical Implementation

- [Resilience4j docs](https://resilience4j.readme.io/){:target="_blank"} – Retry and CircuitBreaker modules. Error threshold configuration (e.g., 50% errors → stop asking for 30 seconds).
- [Spring WebClient docs](https://docs.spring.io/spring-framework/reference/web/webflux-webclient.html){:target="_blank"} – connection and read timeout configuration.
- [OkHttp](https://square.github.io/okhttp/){:target="_blank"} – alternative to WebClient.

### Architecture

- **"Enterprise Integration Patterns"** – chapter on Adapter Pattern. Isolating provider's data format from business logic.
- [Refactoring.Guru – Adapter Pattern](https://refactoring.guru/design-patterns/adapter){:target="_blank"} – visual explanation of creating an interface that hides provider API specifics.

### Testing

- [WireMock – Simulating Faults](https://wiremock.org/docs/simulating-faults/){:target="_blank"} – testing scenarios: timeout (API responds after 10s), connection drop mid-response, 503 error.

### Observability

- [Micrometer docs](https://micrometer.io/docs){:target="_blank"} – HTTP client metrics (errors, response times). `provider_name` tag for future dashboard.

## Completion Criteria

- ☐ Client doesn't stop working when API doesn't respond
- ☐ Retry works with backoff and jitter
- ☐ Circuit breaker opens on too many errors
- ☐ Bulkhead limits concurrent calls
- ☐ Metrics show integration health (including P95/P99)
- ☐ Configuration is external (change without recompilation)
- ☐ Model separation: business logic doesn't depend on provider's JSON structure
- ☐ Separate timeouts for connect and read
- ☐ WireMock tests cover edge cases (malformed JSON, empty responses)
- ☐ I can justify every design decision

## Application at Work

In parallel, I plan to:
- Apply these patterns in current integration project
- Add metrics and MDC logging to existing client
- Analyze current timeouts and retry policy

---

Next post after first results.
