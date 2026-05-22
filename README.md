# Magenv

The simplest Magento 2 local environment. One command to install, zero configuration headaches.

> Clone once per project. One store per instance.

---

## What's included

- **Nginx** · **PHP FPM** · **MySQL** · **OpenSearch** · **Mailpit** · **Xdebug 3**
- Composer, Node.js, npm, Grunt, Git — all inside the PHP container
- PHP version configurable via `.env`
- Xdebug off by default, toggle with `x` inside the container
- Email capture at `http://localhost:8025`

---

## Getting started

```bash
git clone https://github.com/your-org/magenv my-store
cd my-store
docker compose up -d
./bin/install
```

The install script will ask:
- Magento official or Mage-OS
- Version (or latest)
- Luma or Hyvä theme
- Sample data

Everything else is automatic.

> No `/etc/hosts` needed — `.localhost` resolves to `127.0.0.1` automatically.

---

## Accessing your store

| | URL |
|---|---|
| Store | `http://<STORE_PREFIX>.localhost/` |
| Admin | `http://<STORE_PREFIX>.localhost/admin` |
| Mailpit | `http://localhost:8025` |

Credentials are defined in `.env`.

---

## Commands

| Script | What it does |
|---|---|
| `./bin/install` | Full Magento installation |
| `./bin/stop` | Stop containers |
| `./bin/down` | Remove containers |
| `./bin/shell-php` | Shell into the PHP container |
| `./bin/shell-db` | MySQL CLI |
| `./bin/import-db` | Import `dumps/import.sql` |

### Running Magento CLI

```bash
./bin/shell-php
bin/magento cache:flush
```

### Importing a database

Place your dump at `dumps/import.sql` and run:

```bash
./bin/import-db
```

---

## Configuration

All settings live in `.env`:

```env
STORE_PREFIX=mystore
PHP_VERSION=8.3-fpm
MYSQL_ROOT_PASSWORD=root
MAGENTO_ADMIN_USER=admin
MAGENTO_ADMIN_PASSWORD=admin123
```
