# iaop-notification-service

Email, SMS, messaging, notification templates.

## Purpose

iaop-notification-service is part of the **Insurance Agent Operating Platform (IAOP)**, an enterprise
insurance agent lifecycle platform.

## Bounded Context / Ownership

- **Bounded context:** Notifications
- **Owner:** AGENTAPR
- **Group:** platform-service

## Owned Database

notification_db

Each service owns its persistence. No cross-service database access, no shared domain
entities, and no cross-database foreign keys.

## Technical Stack

Rust, Axum, Tokio, PostgreSQL, OpenTelemetry

## Architecture Principles

- Domain-Driven Design (DDD) with explicit bounded contexts
- Hexagonal architecture (ports and adapters)
- Event-driven integration via versioned contracts (see iaop-event-contracts)
- Transactional outbox/inbox for reliable event publication
- PostgreSQL as the system of record where applicable
- Identity and access via Keycloak
- Workflow orchestration via Flowable BPM (see iaop-workflow-service)
- Observability via OpenTelemetry

## Responsibilities

- Notification dispatch
- Email/SMS/messaging channels
- Notification templates
- Delivery tracking

## Non-Responsibilities

- Business event ownership
- Domain logic

## Integration

### Upstream (callers / producers toward this service)

- `All domain microservices`

### Downstream (services this depends on)

- `External providers (SES/Twilio/etc.)`

### Planned Integration Events

- `NotificationRequested`
- `NotificationSent`
- `NotificationFailed`
- `NotificationDelivered`

Event schemas are governed by [iaop-event-contracts](https://github.com/AGENTAPR/iaop-event-contracts).

### Planned API Contracts

- `/api/v1/notifications`
- `/api/v1/templates`

API specifications are governed by [iaop-api-contracts](https://github.com/AGENTAPR/iaop-api-contracts).

## Related Repositories

- [iaop-architecture](https://github.com/AGENTAPR/iaop-architecture) - DDD strategic design, bounded context maps, ADRs, repository landscape
- [iaop-rust-common](https://github.com/AGENTAPR/iaop-rust-common) - Shared Rust technical utilities
- [iaop-test-kit](https://github.com/AGENTAPR/iaop-test-kit) - Shared test infrastructure

## Status

**INITIALIZED - NOT IMPLEMENTED**

This repository has been bootstrapped with baseline documentation and configuration only.
No business logic or service implementation exists yet.

## Development Conventions

- Default branch: `main`
- Development branch: `development` — all changes are committed and pushed here
- Squash merges preferred; merge commits and rebases disabled
- Pull requests required before merging into `main`
- Topics: iaop, insurance, insurance-agents, enterprise-architecture, rust, axum, microservices, notifications, postgresql
## Foundation implementation (2026-11)

Rust/Axum microservice scaffold generated from the canonical IAOP
template (see `iaop-rust-common/templates/axum-microservice` and
`iaop-architecture/docs/standards/IAOP_AXUM_SERVICE_TEMPLATE.md`):

- Crates: `domain`, `application`, `infrastructure`, `api` (+ `src/main.rs`)
- Endpoints: `/health/live`, `/health/ready`, `/api/v1/system/info`, `/openapi.json`, `/docs`
- Persistence: PostgreSQL/SQLx boundary + outbox/inbox migrations
- Security: OIDC/JWT boundary via `iaop-security` (Keycloak)
- Observability: structured JSON logs, correlation IDs, OTLP-ready
- CI: `.github/workflows/main-ci.yml` — fmt/clippy/test on `main` pushes only
- Docs: `docs/architecture.md`, `docs/database.md`, `docs/api.md`

Business features are intentionally not implemented yet.
