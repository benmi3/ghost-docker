# AGENTS.md - Development Guidelines

## Project Overview
This is a comprehensive Docker Compose setup for running Ghost CMS in production with automatic HTTPS, optional analytics, and ActivityPub support. The repository orchestrates multiple services including Ghost, MySQL, Caddy (reverse proxy), and optional Tinybird analytics and ActivityPub federation.

## Architecture
1. **Ghost** - Main CMS application (internal port 2368)
2. **MySQL** - Database backend with health checks and multi-database support
3. **Traefik** - Reverse proxy handling HTTPS/SSL, routing, and external access
4. **Traffic Analytics** (optional) - Tinybird integration for web analytics
5. **ActivityPub** (optional) - Federated social networking support

## Build/Test Commands
- **Lint shell scripts**: `find . -type f -name "*.sh" -exec shellcheck {} +`
- **Start services**: `docker compose up -d`
- **Stop services**: `docker compose down`
- **View logs**: `docker compose logs -f [service]` (ghost, traefik, db, etc.)
- **Check service status**: `docker compose ps`
- **Test single service**: `docker compose up [service]` (e.g., ghost, traefik)
- **Shell access**: `docker compose exec [service] sh`

## Code Style Guidelines
- **Indentation**: 4 spaces (general), 2 spaces (JSON/YAML), tabs (Caddyfile/Makefile)
- **Shell scripts**: Follow shellcheck recommendations, use `set -euo pipefail`
- **JavaScript**: Node.js style, use `const`/`let`, avoid `var`
- **Environment variables**: Use `${VAR:?error}` for required vars, `${VAR:-default}` for optional
- **Docker**: Pin image versions with SHA256 hashes for security
- **File naming**: Use kebab-case for scripts, lowercase for configs
- **Error handling**: Always validate inputs, provide clear error messages with context
- **Comments**: Use `#` for shell, `//` for JS, document complex logic
- **Security**: Never commit secrets, use .env for sensitive data

## Configuration
- **Required variables**: `DOMAIN`, `DATABASE_PASSWORD`, `DATABASE_ROOT_PASSWORD`
- **Ghost config pattern**: `section__subsection__key=value` (e.g., `mail__options__service=Mailgun`)
- **Data persistence**: Volumes stored in `./data/ghost` and `./data/mysql`
- **Key files**: `.env` (main config), `compose.yml` (services), `traefik/traefik.yml` (proxy config)

## Development Workflow
1. Copy `.env.example` to `.env` and configure required variables
2. Run `docker compose up -d` to start services
3. Access Ghost at `https://DOMAIN` (Traefik handles SSL automatically)
4. Monitor with `docker compose logs -f ghost`

## Migration Tools
- `scripts/migrate.sh` - Migrates existing Ghost CLI installations to Docker
- `scripts/config-to-env.js` - Converts Ghost JSON config to .env format