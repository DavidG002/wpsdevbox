# wpsdevbox – WordPress Sandbox (Bedrock)

Development / testing WordPress environment for integrating with your app.  
Domain: **wpsdevbox.com**

Built with [Roots Bedrock](https://roots.io/bedrock/).

## Requirements

- Docker + Docker Compose
- Git

PHP and Composer run **inside Docker**. You do not need them installed on the host.

## Local setup

```bash
cd /home/davidg/Developer/sandbox/wspdevbox

git pull

# 1. Environment file
cp .env.example .env
# Edit .env — strong DB password + salts from https://roots.io/salts.html

# 2. Install PHP dependencies (Composer via Docker)
docker compose run --rm composer install

# 3. Start the stack
docker compose up -d

# 4. Install WordPress (first time only)
docker compose run --rm wpcli core install \
  --url="http://localhost:8080" \
  --title="WPS DevBox" \
  --admin_user="admin" \
  --admin_password="choose_a_strong_password" \
  --admin_email="you@example.com" \
  --skip-email \
  --path=web/wp
```

Site: **http://localhost:8080**  
Admin: **http://localhost:8080/wp/wp-admin/**

## Useful commands

```bash
# Composer (plugins, updates)
docker compose run --rm composer require wpackagist-plugin/polylang
docker compose run --rm composer require wpackagist-plugin/wordpress-seo

# WP-CLI
docker compose run --rm wpcli plugin list --path=web/wp

# Stop
docker compose down
```

`composer` and `wpcli` use the `tools` profile, so they do **not** stay running with `docker compose up -d`. `run --rm` still works.

## Project structure (Bedrock)

```
.
├── config/
├── web/                    # document root
│   ├── app/                # wp-content equivalent
│   ├── wp/                 # WordPress core (Composer)
│   ├── index.php
│   └── wp-config.php
├── docker/
├── .github/workflows/
├── composer.json
├── docker-compose.yml
└── .env                    # never commit
```

## Deployment (GitHub Actions → VPS)

Add these GitHub Actions secrets:
- `SSH_PRIVATE_KEY`
- `SSH_HOST`
- `SSH_USER`
- `DEPLOY_PATH` (e.g. `/var/www/wpsdevbox`)

Push to `main` to deploy.

On the VPS you still need Nginx (document root = `web/`), PHP-FPM 8.3, MariaDB, Let's Encrypt for `wpsdevbox.com`, and a production `.env`.
