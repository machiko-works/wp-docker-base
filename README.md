# wp-docker-base

WordPress local dev template (Docker Compose)

## Structure
- `docker/` : docker-compose + env example
- `wp/` : WordPress files (not included in this repo; project-specific)

## Quick start
1. Create project folder from this template.
2. Copy env:
   - `cp docker/.env.example docker/.env`
   - edit passwords/ports if needed
3. Create `wp/` folder:
   - `mkdir wp`
   - (Option) place WordPress core files here, or let container populate it.
4. Start:
   - `cd docker`
   - `docker compose up -d`
5. Access:
   - WordPress: `http://localhost:WP_PORT`
   - Mailpit: `http://localhost:MAIL_UI_PORT`
   - phpMyAdmin: `http://localhost:PMA_PORT`

## Notes
- `.env` is ignored (secrets).
- Mail: use SMTP Host `mail`, Port `1025` (via WP Mail SMTP plugin).
- WP-CLI runs inside the `wordpress` container (root requires `--allow-root`).