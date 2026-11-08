# Axiom Memory — SPEC (SIGNED 2026-11-08)
OWNS: lib/memory/axiom-memory.ts, lib/memory/axiom-memory.test.ts, supabase/migrations/*_axiom_memory.sql

## Purpose
Production implementation of MBD 20.2 (approved use: persistent, hierarchical, per-tenant workflow context).
Keeps the memory *pattern* (core blocks + archival recall). Drops the runtime: upstream Docker image is unsupported, and its Postgres has no tenant RLS.

## Hard constraints (release-blocking)
1. **Read-only to callers.** Public interface exposes `getCoreBlocks`, `recall` only. No tenant-state mutation (20.2).
2. **FORCE RLS** on every table; `tenant_id` from JWT claim only, never from the request body.
3. **Writes** only through a server-side ingestion job running as the tenant's own role (no `service_role` bypass); every write logged to the audit table.
4. **Embeddings local only** (Batch 04 §1.1). No call to external embedding APIs. Unavailable model → fail closed + Rule 0.2 report.
5. **Pydantic/Zod-validated** inputs and outputs; unknown keys rejected.
6. Region: Supabase **eu-north-1** only.

## Data model
| Table | Columns | Notes |
|---|---|---|
| `memory_core_blocks` | `id, tenant_id, label, value text, char_limit int, updated_at` | small, always-in-context blocks |
| `memory_archival` | `id, tenant_id, source_ref, content text, embedding vector(384), created_at` | pgvector; HNSW index |
| `memory_audit` | `id, tenant_id, actor, op, ref_id, at` | append-only |

All three: `ENABLE` + `FORCE ROW LEVEL SECURITY`; policy `tenant_id = (auth.jwt()->>'tenant_id')::uuid`.

## Interface (`lib/memory/axiom-memory.ts`)
```ts
getCoreBlocks(ctx: TenantCtx): Promise<CoreBlock[]>
recall(ctx: TenantCtx, query: string, k?: number /* ≤ 20 */): Promise<ArchivalHit[]>
```

## Architect decisions (2026-11-08)
- [x] Embeddings: **gte-small**, **384-dim**, run in a **Supabase Edge Function** (built-in model, eu-north-1). Column: `vector(384)`.
- [x] Retention: **contract term + 30 days**. Rows are deleted 30 days after tenant offboarding.
- [x] Build repo: **ai-revenue-systems-site**, branch base `acp/production-readiness`.
