# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A Docker Compose deployment configuration for self-hosted [n8n](https://n8n.io) (workflow automation). There is no application code, build system, or test suite — the entire repository is three files:

- `docker-compose.yml` — single `n8n` service using the `n8nio/n8n:latest` image, exposing port 5678, with workflow/credential data persisted in the `n8n_data` named volume (mounted at `/home/node/.n8n`)
- `.env.example` — template for the `.env` file that docker-compose interpolates (host, protocol, webhook/editor URLs, secure cookie flag, timezone)
- `README.md` — setup instructions

## Common commands

```bash
cp .env.example .env        # then edit values (required before first run)
docker compose up -d        # start n8n
docker compose down         # stop (data persists in the n8n_data volume)
docker compose logs -f n8n  # tail logs
docker compose pull && docker compose up -d  # update to latest image
docker compose config       # validate compose file / check env interpolation
```

## Conventions and constraints

- TLS is not handled here: the stack is designed to run behind a reverse proxy (e.g. Nginx) that terminates HTTPS for the domain in `N8N_HOST`. Do not add TLS/certificate handling to the compose file.
- All environment-specific values live in `.env` (gitignored by convention — only `.env.example` is committed). When adding a new `N8N_*` variable to `docker-compose.yml`, add a corresponding documented placeholder to `.env.example`.
- `N8N_PORT` and `NODE_ENV` are intentionally hardcoded in `docker-compose.yml`; the host port mapping is `5678:5678`.
- Persistent state (workflows, credentials, encryption key) lives in the `n8n_data` volume. Never suggest `docker compose down -v` or removing that volume without explicit confirmation — it destroys all n8n data.
