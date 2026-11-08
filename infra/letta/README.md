# Axiom Memory Runtime (Letta) — R&D SANDBOX ONLY

> 🚫 **NOT FOR PRODUCTION. NO TENANT DATA.** Architect ruling 2026-11-08: production memory = `lib/memory/axiom-memory.ts` on Supabase FORCE RLS (see `.unlazy/axiom-memory/SPEC.md`). This container exists only to evaluate the memory pattern on synthetic data.

Source: https://github.com/letta-ai/letta (Apache-2.0)
Ruling (Architect, 2026-10-29): self-hosted, internal network only, all LLM traffic via `axiom-llm-router` (Module 02 Addendum).
Hardened 2026-11-08: no host port, `internal: true` network, auth on, pinned image required, documented env vars only.

## Required env (`infra/letta/.env`, never committed)
- `LETTA_IMAGE`: pinned `letta/letta:<tag>` or `letta/letta@sha256:<digest>`
- `LETTA_SERVER_PASSWORD`: API bearer secret
- `AXIOM_ROUTER_KEY`: key the router accepts

## Router contract
`axiom-llm-router` must join this network as external and be the only service with egress:
```yaml
networks:
  axiom_internal_network: { external: true }
```

## Known gaps
- Upstream says the Docker image is no longer an actively maintained or supported product surface.
- No embedding env var exists: set `embedding` per agent at creation.
- Persistence uses the bundled Postgres. Set `LETTA_PG_URI` (pgvector required) to move it out.

Run: `docker compose --env-file infra/letta/.env -f infra/letta/docker-compose.yml up -d`
