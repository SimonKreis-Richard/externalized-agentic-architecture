# Example AGENTS.md — Project Titanium: API Integration Dashboard

> *Fictional example of an `AGENTS.md` file for a made-up project. For the real specification, see [ARCHITECTURE.md §5 — The Project Workspace Model](../ARCHITECTURE.md).*

---

## Project Titanium — API Integration Dashboard

**Purpose**: Build a real-time monitoring dashboard for RESTful API integrations across microservices. Provide health checks, latency tracking, and error aggregation.

## Contacts

| Role | Name | Notes |
|------|------|-------|
| Lead Developer | Dr. Helena Vance | Primary technical contact |
| Product Owner | Marcus Chen | Approves scope and priorities |
| DevOps | Aisha Patel | Deployment, CI/CD, secrets |
| Stakeholder | Prof. James Kirkwood | Weekly check-ins |

## Technical Conventions

- **Language**: TypeScript (Node.js 22 LTS)
- **Framework**: Fastify for API layer, React 19 for frontend
- **Database**: PostgreSQL 16 with Drizzle ORM
- **Testing**: Vitest + Playwright (E2E)
- **Deployment**: Docker Compose → AWS ECS (Fargate)
- **CI/CD**: GitHub Actions (`.github/workflows/`)
- **Secrets**: Never hard-code. Use `.env.local` (gitignored) + AWS Secrets Manager

## Known Pitfalls

- The `/api/v2/metrics` endpoint **must not** be called more than once per 30 seconds (rate limit enforced upstream)
- WebSocket reconnection logic has a 3-second exponential backoff — do not override
- `packages/` is a Yarn Workspace monorepo; always use `yarn` (not `npm`) for dependency management
- Local dev requires `docker compose up -d` for Postgres + Redis — see `docker/dev.yml`

## Current Phase

**Phase 2 — Metric Ingestion Pipeline**

- Implement `/api/v1/metrics/ingest` endpoint with batch support
- Design the TimescaleDB hypertable schema for metric storage
- Add WebSocket push for real-time dashboard updates
- Write integration tests for the ingest → store → notify flow

## Data Models (Reference)

```typescript
interface MetricEvent {
  service: string;        // e.g., "user-service"
  endpoint: string;       // e.g., "/api/v1/users"
  method: "GET" | "POST" | "PUT" | "DELETE";
  statusCode: number;
  latencyMs: number;
  timestamp: string;      // ISO 8601
  error?: {
    code: string;
    message: string;
  };
}
```

## Quick Start

```bash
git clone git@github.com:example-org/project-titanium.git
cd project-titanium
yarn install
docker compose -f docker/dev.yml up -d
cp .env.example .env.local   # edit secrets
yarn dev
```

## Changelog

| Date | Change |
|------|--------|
| 2026-05-10 | Phase 2 kickoff — metric ingestion pipeline |
| 2026-04-28 | Phase 1 complete — API gateway scaffold |
| 2026-04-15 | Repository initialized |

---

> **Estimated token cost**: ~450 tokens. Under the 500-token target.
