# ServiCenter — Architecture Notes

These notes describe a **sanitized conceptual view** of the architecture. The implementation is private, and the diagram is not intended as a complete deployment map.

## Application layers

1. **Presentation:** Angular/TypeScript frontend, with client-side routing, HTTP calls, and lazy-loaded pages.
2. **API:** Node.js/Express exposes REST endpoints and applies authentication, validation, and business rules.
3. **Persistence:** application logic reads and writes relational data through the database layer.
4. **Storage:** Turso/libSQL for production; local SQLite for development, tests, import/export, and backups.

![Architecture](../assets/architecture.svg)

## Domain model

Main documented entity groups include customers, vehicles, appointments, service intake, estimates, and separate labor/parts line items. Relationships, foreign keys, and indexes support consistent linked records. Relevant historical documents preserve snapshots of customer and vehicle information so later edits do not overwrite the historical context.

> The entity names and relationships here are conceptual; this file deliberately omits private schema definitions, endpoint inventories, and deployment configuration.

## Operational consistency

Changes can affect several connected records. For example, finalizing an estimate affects the corresponding service workflow. Transactional operations reduce the risk of partial updates when several records must change together.

## Database-side processing

The application includes optimizations for searches, estimate filtering, revenue aggregation, and backup statistics directly in Turso/SQL. Limiting reads and writes to relevant entities helps avoid unnecessary processing and repeated full-collection loading.

## Scope & limitations

This document summarizes the project's recorded architecture, not an independently audited security review, performance benchmark, or public implementation. No production URLs, credentials, private schemas, or customer data are included.
