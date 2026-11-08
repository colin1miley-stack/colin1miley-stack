# GATES — Axiom Memory
- [x] G0 Architect signs SPEC.md + doc 20.2 amendment
- [x] G1 Migration: ENABLE + FORCE RLS on all 3 tables; Supabase advisors clean (ai-revenue-systems-site PR #73, 2026-11-08)
- [x] G2 Cross-tenant test: tenant A context gets 0 rows from tenant B (core, archival, audit). PASS locally and on Supabase staging; negative control fails as expected
- [x] G3 Zero external embedding calls: embedder is the Supabase Edge Function only. Static guard test passes; staging probe: 384-dim, unit norm, deterministic; 401 without key (PR #73)
- [x] G4 Interface exposes no write method; contract test pins the exports (PR #73)
- [x] G5 Schema validation rejects extra keys, on inputs and DB rows (PR #73)
- [ ] G6 Wired into the context-enrichment path, read-only
- [ ] G7 Retention job deletes a tenant's rows 30 days after offboarding (test with a fixed clock)
