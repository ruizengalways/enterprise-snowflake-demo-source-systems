# Enterprise Snowflake Demo Source Systems

Deterministic external source-system simulator for the Enterprise Snowflake reference platform.

This repository represents **the world outside the Snowflake data platform**. A real company adopting the platform/framework would not normally copy this repository.

## Boundary

```text
external demo source runtime
        ↓
source-specific transport / ingestion
        ↓
project BRONZE contract
──────────────────────────────── downstream processing boundary
Silver -> Gold -> semantic
```

This repository owns only the external side of that boundary. It must never contain:

- dbt transformations;
- Silver canonical/history maintenance;
- marts or Semantic Views;
- downstream reconciliation/business-quality logic;
- Framework processing checkpoints;
- `PLATFORM_CONTROL` ownership;
- platform Terraform/RBAC ownership.

Connector-owned positions such as SQL Server LSNs, Kafka offsets and API cursors remain in the source/ingestion path. They are not Framework processing checkpoints.

## Relationship to domain `standalone/` simulators

Transport and Health intentionally keep small deterministic simulators inside their own `standalone/` directories. Those files have a different purpose and are **not** a second enterprise source-runtime implementation.

```text
domain repo standalone/
  -> self-contained contract/portability fixture
  -> proves the domain can demonstrate its source contract without Framework,
     PLATFORM_CONTROL, Terraform or enterprise WIF
  -> remains deliberately small and domain-local

this demo-source repository
  -> integration/source-runtime simulator
  -> eventually drives real ingestion technology comparisons
  -> may expose SQL/file/event/Kafka/streaming source interfaces
```

The same logical scenarios may exist in both places, but the ownership is different. A domain portability fixture should not grow Kafka producers, connector lifecycle state or external service orchestration. When a reusable external source runtime is implemented, that runtime belongs here and the domain fixture remains a compact independent proof.

## Target responsibilities

As implementation is introduced, this repository may own:

- deterministic synthetic Health and Transport source data;
- continuous/event data generation;
- controlled source mutations;
- CSV/file and SQL-source simulation;
- optional public streaming adapters;
- Kafka producers;
- direct Snowpipe Streaming producers;
- source outage, duplicate, late-arrival, out-of-order, delete, schema-change and volume-spike scenarios.

These are **target capabilities**, not a claim that every item is implemented today. Do not create placeholder directories before the first real implementation file exists.

## Current phase boundary

Broad source/streaming implementation remains deliberately deferred until live DEV platform/WIF and downstream Framework behavior are proven end to end.

In particular, Kafka Connector, direct Snowpipe Streaming and Openflow are not current implementation work. Their implementation begins only after the live acceptance gate tracked in `enterprise-snowflake-platform-infra` is complete enough to provide a real DEV consumer.

Until then, this repository remaining documentation-only is intentional rather than incomplete scaffolding.

## Transport ingestion comparison rule

The same logical Transport event dataset will later drive both ingestion paths:

```text
Transport external generator
  -> direct Snowpipe Streaming
  -> project BRONZE contract
```

and:

```text
Transport external generator
  -> Kafka
  -> Snowflake Kafka Connector
  -> project BRONZE contract
```

This keeps ingestion technology outside the downstream data-model contract. Switching transport mechanism must not require redesign of Silver/Gold models.

Producer/runtime code belongs here. Snowflake/project-specific ingestion configuration belongs with the consuming domain/platform integration as appropriate; downstream business processing still begins from the same Bronze contract.

## Canonical project status

For current architecture and the next live execution gate, read:

```text
enterprise-snowflake-platform-infra/docs/CURRENT_CONTEXT.md
enterprise-snowflake-platform-infra/docs/PROJECT_BLUEPRINT.md
enterprise-snowflake-platform-infra issue #6 (live DEV/WIF acceptance)
```
