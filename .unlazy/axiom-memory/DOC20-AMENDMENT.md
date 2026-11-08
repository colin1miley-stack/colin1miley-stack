# Doc 20.2 amendment — DRAFT for Architect sign-off (2026-11-08)
**20.2 Axiom Memory (pattern adapted from Letta)**
- Approved use case: unchanged (persistent, hierarchical, per-tenant workflow context; read-only retrieval).
- **Implementation:** in-house `lib/memory/axiom-memory.ts` on Supabase eu-north-1 with FORCE RLS + pgvector. The upstream runtime is **not** deployed in production.
- **Reason:** upstream Docker image is no longer a supported product surface, and its bundled Postgres sits outside tenant RLS.
- **Upstream runtime status:** R&D sandbox only (`infra/letta/`), synthetic data, no tenant data.
- **Embeddings:** local models only (Batch 04 §1.1).
- Status: APPROVED-NOT-INTEGRATED → IN-BUILD on sign-off.
