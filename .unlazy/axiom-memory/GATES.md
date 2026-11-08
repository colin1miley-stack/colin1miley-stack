# GATES — Axiom Memory
- [x] G0 Architect signs SPEC.md + doc 20.2 amendment
- [ ] G1 Migration: ENABLE + FORCE RLS on all 3 tables; Supabase advisors clean
- [ ] G2 Cross-tenant test: tenant A JWT gets 0 rows from tenant B (core, archival, audit)
- [ ] G3 Zero external embedding calls: embedder is the Supabase Edge Function only (network test + static grep)
- [ ] G4 Interface exposes no write method; contract test pins the exports
- [ ] G5 Schema validation rejects extra keys
- [ ] G6 Wired into the context-enrichment path, read-only
- [ ] G7 Retention job deletes a tenant's rows 30 days after offboarding (test with a fixed clock)
