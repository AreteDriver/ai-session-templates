# Example Work-Specific Template Request

A filled-in meta-template showing how to request a new reusable session template for blockchain event ingestion pipeline work.

```md
# Template Builder

## Work Type
Blockchain event ingestion pipeline — polling on-chain events from an RPC endpoint, normalizing them into domain models, persisting to a local database, and triggering downstream actions (detection rules, webhooks, UI updates).

## Main Goal
Create a reusable session template for any work that involves building, debugging, or extending a blockchain event ingestion pipeline. Success means the template captures the specific risks, inputs, and validation steps that apply to this kind of work — not generic data pipeline advice.

## Typical Inputs
- RPC endpoint URL and network (e.g., `fullnode.testnet.sui.io:443`)
- Package ID or contract address to monitor
- Event types to ingest (e.g., `KillMailEvent`, `FuelEvent`, `GateActivatedEvent`)
- Event schema (Move struct definitions or ABI fragments)
- Cursor/checkpoint state from previous ingestion runs
- Target database schema (SQLite or Postgres tables for normalized events)
- Downstream consumers (detection rules, webhook dispatch, map updates)

## Typical Risks
- **Cursor pruning**: fullnodes prune old transactions — stored cursors become invalid and ingestion silently stops. Must detect stale cursors and recover
- **Network mismatch**: testnet vs mainnet package IDs look identical but return `notExists` on the wrong network. Cost: days of silent no-data before you notice
- **Event schema drift**: on-chain contract upgrades change event fields without warning. Ingestion breaks or silently drops new fields
- **Rate limiting**: RPC endpoints throttle or block aggressive polling. Must implement backoff and respect rate limits
- **Duplicate events**: replaying from a recovered cursor can re-ingest events already in the database. Dedup by event ID or transaction digest is mandatory
- **Noisy events**: some event types fire at high volume on passive state changes (e.g., fuel burn ticks on all online assemblies). Without filtering, these overwhelm detection rules with false positives
- **BigInt handling**: on-chain coordinates and IDs use uint256 with offsets (e.g., `1 << 255`). JavaScript loses precision without explicit `BigInt` handling

## Required Constraints
- Ingestion must be idempotent — reprocessing the same event range produces no duplicates
- Cursor state must persist to disk (SQLite or file) — never rely on in-memory state surviving restarts
- Event normalization must happen at ingestion time, not query time
- Failed ingestion runs must not advance the cursor
- Pipeline must log enough context to diagnose silent data gaps (last cursor, last event timestamp, events ingested per poll)
- Do not couple ingestion logic to downstream consumers — use a service boundary

## Required Output Structure
1. Copy-paste-ready session template in markdown
2. Checklist of pre-session questions to answer before starting work
3. Validation steps specific to blockchain ingestion (not generic test advice)

## Validation Expectations
- Verify cursor recovery: simulate a stale cursor, confirm pipeline detects and resets
- Verify dedup: replay an event range, confirm no duplicate rows in target tables
- Verify network correctness: confirm package ID resolves on the target network before starting ingestion
- Verify schema handling: ingest an event with an unexpected field, confirm it does not crash the pipeline
- Verify rate limit behavior: confirm backoff kicks in and pipeline resumes after throttling
- Monitor ingestion lag: time between on-chain event emission and database persistence should be under 30 seconds for polling intervals of 10 seconds

## Final Task
Generate the reusable session template. It should be directly usable for any session involving:
- Building a new event ingestion pipeline from scratch
- Adding a new event type to an existing pipeline
- Debugging silent data gaps or cursor failures
- Migrating an ingestion pipeline to a new network or contract version

The template should assume a Python/FastAPI stack with SQLite storage, but the structure should be adaptable to other stacks. Include the pre-session checklist as a separate section within the template.
```
