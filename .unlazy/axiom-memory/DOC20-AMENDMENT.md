# Doc 20.2 amendment — SIGNED by Architect 2026-11-08
**20.2 Axiom Memory (pattern adapted from Letta)**
- Approved use case: unchanged (persistent, hierarchical, per-tenant workflow context; read-only retrieval).
- **Implementation:** in-house `lib/memory/axiom-memory.ts` on Supabase eu-north-1 with FORCE RLS + pgvector. The upstream runtime is **not** deployed in production.
- **Reason:** upstream Docker image is no longer a supported product surface, and its bundled Postgres sits outside tenant RLS.
- **Upstream runtime status:** R&D sandbox only (`infra/letta/`), synthetic data, no tenant data.
- **Embeddings:** gte-small (384-dim) in a Supabase Edge Function, eu-north-1. Local only (Batch 04 §1.1).
- **Retention:** contract term + 30 days.
- Status: **IN-BUILD** (signed 2026-11-08).
