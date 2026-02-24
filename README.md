# wp-docker-base

WordPress local development template using Docker Compose.

This template is designed for real-world WordPress client projects,
including database migration and uploads handling.

---

## Structure

wp-docker-base/
  docker/
    docker-compose.yml
    .env.example
  wp/ (created per project, not included in this repo)

- docker/ : Docker configuration
- wp/ : WordPress core + content (project-specific, not version controlled)

---

## Quick Start

1. Copy template to a client project

cp -R ~/dev/templates/wp-docker-base ~/dev/clients/client-a/site

2. Setup environment file

cd docker
cp .env.example .env

Edit passwords/ports if necessary.

3. Create wp directory

mkdir ../wp

4. Start containers

docker compose up -d

5. Populate WordPress core (if wp is empty)

docker compose exec wordpress bash -lc 'cp -a /usr/src/wordpress/. /var/www/html/'

6. Access

WordPress: http://localhost:8080
Mailpit: http://localhost:8025
phpMyAdmin: http://localhost:8081

---

## WordPress Installation (CLI)

docker compose exec wordpress wp core install \
  --url="http://localhost:8080" \
  --title="Project Title" \
  --admin_user="admin" \
  --admin_password="password" \
  --admin_email="admin@example.com" \
  --skip-email \
  --allow-root

---

## Real Project Migration Flow

1. Import production database

docker compose exec -T db sh -lc '
mysql -uroot -p"$MYSQL_ROOT_PASSWORD" "$MYSQL_DATABASE"
' < db.sql

2. Replace production URL

docker compose exec wordpress wp search-replace \
'https://example.com' \
'http://localhost:8080' \
--all-tables --precise --allow-root

---

## Uploads Handling

Copy production uploads:

cp -R production/wp-content/uploads ../wp/wp-content/

---

## Mail Testing

Use WP Mail SMTP plugin:

SMTP Host: mail
Port: 1025
Encryption: None
Authentication: Off

Mailpit UI: http://localhost:8025

---

## Stop / Remove

Stop containers:

docker compose down

Remove containers + volumes:

docker compose down -v

---

## Common Issues

Forbidden (403)
wp/ directory is empty. Run:

docker compose exec wordpress bash -lc 'cp -a /usr/src/wordpress/. /var/www/html/'

DB not connecting
Check .env values and container status:

docker compose ps

---

## Notes

.env is ignored (contains secrets)
wp/ is project-specific and not version controlled
WP-CLI requires --allow-root inside container