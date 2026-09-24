# wpsdevbox – WordPress Sandbox (Bedrock)

Development / testing WordPress environment for integrating with your app.  
Domain: **wpsdevbox.com**

Built with [Roots Bedrock](https://roots.io/bedrock/).

## Requirements

- PHP ≥ 8.1 (8.3 recommended)
- Composer
- Docker + Docker Compose (for local development)
- Git

## Local setup

```bash
cd /home/davidg/Developer/sandbox/wspdevbox

# 1. Install PHP dependencies
composer install

# 2. Environment file
cp .env.example .env
# Edit .env – especially DB_* and generate real salts from https://roots.io/salts.html

# 3. Start the stack
docker compose up -d

# 4. Install WordPress (first time only)
docker compose exec wordpress bash
wp core install \
  --url="http://localhost:8080" \
  --title="WPS DevBox" \
  --admin_user="admin" \
  --admin_password="choose_a_strong_password" \
  --admin_email="you@example.com" \
  --skip-email
```

Site: **http://localhost:8080**  
Admin: **http://localhost:8080/wp/wp-admin/**

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

## Plugins later

```bash
composer require wpackagist-plugin/polylang
composer require wpackagist-plugin/wordpress-seo
```
