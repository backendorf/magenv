# Magenv

The simplest Magento 2 local environment. One command to install, zero configuration headaches.

> Clone once per project. One store per instance.

---

## What's included

- **Nginx** · **PHP FPM** · **MySQL** · **OpenSearch** · **Mailpit** · **Xdebug 3**
- Composer, Node.js, npm, Grunt, Git — all inside the PHP container
- PHP version configurable via `.env`
- Xdebug off by default, toggle with `x` inside the container
- 2FA disabled by default (dev environment)
- Email capture at `http://mailpit.localhost`

---

## Getting started

```bash
git clone https://github.com/your-org/magenv my-store
cd my-store
./magenv install
```

The install script will ask:
- Store prefix
- Distribution: Magento official or Mage-OS
- Version (or latest)
- Theme: **Hyvä** (default) or Luma
- Sample data

Everything else is automatic. Containers are started for you.

> No `/etc/hosts` needed — `.localhost` resolves to `127.0.0.1` automatically.

---

## Accessing your store

| | URL |
|---|---|
| Store | `http://<STORE_PREFIX>.localhost/` |
| Admin | `http://<STORE_PREFIX>.localhost/admin` |
| Mailpit | `http://mailpit.localhost` |

Credentials are defined in `.env`.

---

## Commands

Use `./magenv <command>` from the project root:

| Command | What it does |
|---|---|
| `./magenv install` | Full Magento installation |
| `./magenv up` | Start containers |
| `./magenv stop` | Stop containers (data preserved) |
| `./magenv down` | Remove containers (volumes preserved) |
| `./magenv php` | Shell into the PHP container |
| `./magenv db` | MySQL CLI |
| `./magenv import-db` | Import `dumps/import.sql` into the database |

### Magento CLI

```bash
./magenv php
bin/magento cache:flush
bin/magento setup:upgrade
```

### Database import

Place your dump at `dumps/import.sql` and run:

```bash
./magenv import-db
```

---

## Configuration

All settings live in `.env`:

```env
# Project
STORE_PREFIX=mystore
MAGENTO_BASE_URL=http://${STORE_PREFIX}.localhost/

# PHP
PHP_VERSION=8.3-fpm
PHP_MEMORY_LIMIT=4G

# Database
MYSQL_ROOT_PASSWORD=root
MYSQL_DATABASE=${STORE_PREFIX}_db

# Admin
MAGENTO_ADMIN_USER=admin
MAGENTO_ADMIN_PASSWORD=admin123
MAGENTO_ADMIN_FRONTNAME=admin
```

---

<sub>Made with 🇧🇷 in Brazil.</sub>
