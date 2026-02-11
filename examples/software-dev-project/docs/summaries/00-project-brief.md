# Project Brief: Task Queue API
**Created:** 2026-02-08
**Last Updated:** 2026-02-08

## Project Owner
- **Name:** Alex (solo developer)
- **Domain:** Backend web services
- **Context:** Personal side project, building a task queue API in Go

## Project
- **Type:** Software development
- **Scope:** Build a REST API for distributed task queue management with worker pools, retry logic, and dead-letter handling. Backed by PostgreSQL.
- **Target Deliverable:** Working Go module with tests and Docker setup
- **Timeline:** ~2 weeks (evenings/weekends)

## Input Documents
| Document | Path | Processed? | Summary At |
|----------|------|-----------|------------|
| API design notes | `docs/discovery/api-notes.md` | Yes | `docs/summaries/source-api-notes.md` |
| Competitor analysis | `docs/research/existing-queues.md` | Yes | `docs/summaries/source-existing-queues.md` |

## Success Criteria
- Queue operations (enqueue, dequeue, ack, nack) under 10ms p99
- At-least-once delivery guarantee
- Horizontal worker scaling without coordination
- 90%+ test coverage on core queue logic

## Known Constraints
- PostgreSQL only (no Redis dependency — keep infrastructure simple)
- Must run in a single Docker Compose stack for local dev
- Go standard library preferred over heavy frameworks

## Project Phase Tracker
| Phase | Status | Summary File | Date |
|-------|--------|-------------|------|
| Discovery & Design | Complete | `docs/summaries/source-api-notes.md` | 2026-02-08 |
| Core Implementation | Complete | `docs/summaries/handoff-2026-02-09-core-impl.md` | 2026-02-09 |
| Worker & Retry Logic | Not Started | | |
| Tests & Docker | Not Started | | |
