# Enterprise Snowflake Demo Source Systems

Deterministic source-system simulator for the Enterprise Snowflake reference platform.

This repository represents **the world outside the Snowflake data platform**. A real company adopting the platform framework would not normally copy this repository.

## This repository owns

- deterministic synthetic Health and Transport source data
- continuous/event data generation
- controlled source mutations
- CSV/file source simulation
- SQL Server simulation
- optional public streaming adapters
- Kafka producers
- direct Snowpipe Streaming producers
- source outage, duplicate, late-arrival, out-of-order, delete, schema-change and volume-spike scenarios

## Boundary

Responsibility ends at the source/RAW boundary.

This repository must never contain:

- dbt transformations
- canonical models
- marts
- semantic views
- downstream reconciliation logic

The same logical Transport event dataset will later drive direct Snowpipe Streaming and Kafka Connector paths so ingestion mechanisms can be compared without redesigning downstream models.

Openflow, Kafka and Snowpipe Streaming are **not implemented during Phase 0**.

The canonical platform architecture is maintained in `enterprise-snowflake-platform-infra/docs/PROJECT_BLUEPRINT.md`.