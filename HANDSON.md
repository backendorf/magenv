# 🚀 Magento 2 Docker: Hands-on Guide

This practical guide will take you from zero to a functional Magento 2 store running on your local Docker.

---

> [!NOTE]
> **Single-Store Design**: This project is intended to be cloned for each new Magento installation. It is not designed to host multiple stores within the same Docker setup.

## 🛠️ Prerequisites
- Docker and Docker Compose installed.
- Git installed.

---

## 🏁 Step 1: Initial Configuration

1. **Clone the repository** (if you haven't already):
   ```bash
   git clone <repository-link>
   cd docker-magento-2
   ```

2. **Edit the `.env` file**:
   The `.env` file now centralizes all configurations. You can change the PHP version, memory limits, and even Magento access credentials.
   ```env
   # --- PROJECT ---
   STORE_PREFIX=mystore
   MAGENTO_BASE_URL=http://${STORE_PREFIX}.localhost/

   # --- ENVIRONMENT ---
   PHP_VERSION=8.3-fpm
   PHP_MEMORY_LIMIT=4G

   # --- DATABASE ---
   MYSQL_ROOT_PASSWORD=root
   MYSQL_DATABASE=${STORE_PREFIX}_db
   ```
   > [!TIP]
   > The `./install` script will automatically use the values you define here!

---

## 🐳 Step 2: Starting the Containers

Start the Docker environment:
```bash
docker-compose up -d
```

This command will download the necessary images and start the services.

---

## 📦 Step 3: Installing Magento 2

Now that the containers are running, let's install Magento!

1. **Run the automatic installation script from the host**:
   The environment comes with a ready-to-use script in the project root:
   ```bash
   ./install
   ```
   This script will ask which Magento version you want to install (or default to latest), configure the database, create the admin user, and prepare OpenSearch. It handles all communication with Docker for you.

---

## 🌍 Step 4: Accessing Your Store

After installation, you can access your store. 

> [!TIP]
> **No `etc/hosts` needed!** Since we use the `.localhost` TLD, your browser and system will automatically resolve these domains to `127.0.0.1` without any manual configuration.

After installation, you can access:

- **Frontend**: `http://<STORE_PREFIX>.localhost/`
- **Admin**: `http://<STORE_PREFIX>.localhost/admin`
  - **User**: `admin`
  - **Password**: `admin123`
- **Mailpit**: `http://localhost:8025/` (to view emails sent by the store)

---

## ⚡ Daily Workflow

### Toggle Xdebug
In the PHP container, use the simplified command to turn Xdebug on or off:
- `x`: Enables or disables Xdebug automatically.

### Magento Commands
Always run Magento commands inside the PHP container:
```bash
docker-compose exec php bin/magento cache:flush
docker-compose exec php bin/magento setup:upgrade
```

### Logs
To see what's happening:
```bash
docker-compose logs -f
```

---

## 🗄️ Database Import

If you need to import an existing database, follow these steps:

1. **Place your `.sql` file in the `dumps/` folder and rename it to `import.sql`**.

2. **Run the automatic import script from the host**:
   ```bash
   ./import-db
   ```
   *(The script will automatically use the root credentials and database name from your `.env` file)*.

---

## 🛑 Finishing

To stop working and turn everything off:
```bash
docker-compose down
```

> [!IMPORTANT]
> Your MySQL data is persisted in the `./mysql/data` folder.
