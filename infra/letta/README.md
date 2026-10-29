# Letta (agent memory runtime)
Source: https://github.com/letta-ai/letta (Apache-2.0)
Ruling (Architect, 2026-10-29): self-hosted via `docker-compose.yml`, internal network only, all LLM + embedding traffic through `axiom-llm-router` (Module 02 Addendum).
Run: `docker compose -f infra/letta/docker-compose.yml up -d`
