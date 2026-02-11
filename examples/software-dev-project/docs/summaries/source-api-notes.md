# Source Summary: API Design Notes
**Processed:** 2026-02-08
**Source Path:** `docs/discovery/api-notes.md`
**Document Type:** requirements
**Confidence:** high

## Exact Numbers & Metrics
- Target p99 latency: 10ms for queue operations
- Max message size: 256KB
- Default visibility timeout: 30 seconds
- Max retry count: 5 (configurable per queue)
- Dead-letter threshold: retry count exceeded OR message age > 24 hours

## Key Facts (Confirmed)
- Messages are JSON payloads with user-defined schema — stated in "Message Format" section
- Each queue is independent (no cross-queue transactions) — stated in "Scope" section
- Worker dequeue is pull-based, not push — stated in "Worker Model" section

## Requirements & Constraints
- REQUIREMENT: At-least-once delivery
  - CONDITION: Always — no exactly-once mode
  - CONSTRAINT: Duplicates possible; consumers must be idempotent
  - PRIORITY: must-have

- REQUIREMENT: Visibility timeout on dequeue
  - CONDITION: When a worker dequeues a message, it becomes invisible to other workers
  - CONSTRAINT: IF not acked within timeout, THEN message returns to queue
  - PRIORITY: must-have

- REQUIREMENT: Dead-letter queue
  - CONDITION: IF retry count > max OR message age > 24h
  - CONSTRAINT: Dead-letter messages must preserve original metadata + failure reason
  - PRIORITY: must-have

## Decisions Referenced
- DECISION: PostgreSQL SKIP LOCKED for dequeue instead of SELECT FOR UPDATE
  - RATIONALE: Avoids lock contention under concurrent workers, benchmarks show 3x throughput
  - ALTERNATIVES MENTIONED: Redis-backed queue, SELECT FOR UPDATE with NOWAIT
  - DECIDED BY: Alex

## Relationships to Other Documents
- SUPPORTS: competitor analysis findings on Postgres-backed queue viability
- DEPENDS ON: none (this is the root design doc)

## Open Questions & Ambiguities
- UNCLEAR: Should failed messages preserve partial worker output? — needs decision
- ASSUMED: Single PostgreSQL instance is sufficient for MVP — revisit if throughput > 10K msg/s

## Verbatim Quotes Worth Preserving
- "Keep it boring. Postgres does 95% of what people use Redis for in queues, without the operational overhead." — Alex, design rationale
