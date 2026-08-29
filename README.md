# Enterprise Snowflake Demo Source Systems

Deterministic source-system simulator for the Enterprise Snowflake reference platform.

This repository represents **the world outside the Snowflake data platform**. A real company adopting the platform/framework would not normally copy this repository.

## Boundary

Responsibility ends at the source/project-RAW boundary.

This repository must never contain:

- dbt transformations;
- canonical models;
- marts;
- Semantic Views;
- downstream reconciliation/business-quality logic;
- platform Terraform/RBAC ownership.

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

The platform/framework foundation is still awaiting live DEV control-plane proof. Broad source/streaming implementation therefore remains deliberately deferred.

In particular, Kafka Connector, direct Snowpipe Streaming and Openflow are not current implementation work. They begin only after remote state, Snowflake WIF, DEV platform apply, project delivery and live framework behavior are proven.

## Transport comparison rule

The same logical Transport event dataset will later drive both ingestion paths:

```text
Transport generator -> direct Snowpipe Streaming -> project RAW contract
```

and:

```text
Transport generator -> Kafka -> Snowflake Kafka Connector -> project RAW contract
```

This keeps ingestion technology outside the downstream data-model contract. Switching transport mechanism must not require redesign of staging/canonical/marts.

Producer/runtime code belongs here. Snowflake/project-specific ingestion configuration belongs in `enterprise-snowflake-transport-analytics` when implementation begins.

## Canonical project status

For current architecture, verified CI and the next live execution gate, read:

```text
enterprise-snowflake-platform-infra/docs/CURRENT_CONTEXT.md
enterprise-snowflake-platform-infra/docs/PROJECT_BLUEPRINT.md
```
