# Self-hosted n8n (Docker)

This repository contains a reproducible Docker setup for running n8n.

## What this includes
- Docker Compose setup for n8n
- Persistent data storage via Docker volumes
- Environment-based configuration
- Designed to run behind a reverse proxy (e.g. Nginx)
- Importable workflows in `workflows/` (see `docs/` for setup guides)

## Workflows
- **Google Search Console weekly SEO report** — pulls top queries and pages
  for your site every Monday. Setup guide: `docs/google-search-console-setup.md`

## Prerequisites
- Linux server (Ubuntu recommended)
- Docker and Docker Compose
- Domain pointing to the server
- HTTPS handled by a reverse proxy

## Setup
1. Clone the repository
2. Copy `.env.example` to `.env` and update values
3. Run:
```bash
docker compose up -d
