# 🚀 Laravel Docker Starter Kit

A **zero-setup Laravel development environment** using Docker.

Just clone and run — Laravel installs automatically with MySQL, Redis, Nginx, Mailhog, and phpMyAdmin ready out of the box.

Perfect for fast local Laravel development using:

* PHP 8.4
* Laravel
* MySQL 8
* Redis
* Nginx
* Mailhog
* phpMyAdmin

---

# ⚡ Features

* PHP 8.4 (FPM)
* Nginx (fast web server)
* MySQL 8 (database)
* Redis (cache, queue, sessions)
* Mailhog (email testing)
* phpMyAdmin (database UI)
* Xdebug (installed, disabled by default)
* Automatic Laravel installation
* Automatic `.env` setup
* Automatic dependency installation
* Automatic app key generation
* Automatic database migration
* Automatic permission fixing

This is designed for **real developer productivity**.

No wasted setup time.

---

# 🎯 Goal

No manual Laravel setup required.

Just run:

```bash
git clone <your-repository>
cd <your-project>
docker compose up --build
```

That’s it.

Laravel installs automatically.

---

# 🧠 What Happens Automatically

On first container startup:

* Laravel is installed (if not already present)
* `.env.docker` is copied to `.env`
* Composer dependencies are installed
* Application key is generated
* Storage permissions are fixed
* MySQL connection is verified
* Database migrations are executed
* Config cache is cleared

You do NOT need to manually run:

```bash
composer install
php artisan key:generate
php artisan migrate
```

Everything is handled automatically.

---

# 📦 Docker Services

This setup includes:

| Service      | Purpose                        |
| ------------ | ------------------------------ |
| `app`        | PHP-FPM + Laravel application  |
| `nginx`      | Web server                     |
| `mysql`      | MySQL database                 |
| `redis`      | Redis cache / queue / sessions |
| `mailhog`    | Local email testing            |
| `phpmyadmin` | Database management UI         |

---

# 🌐 Ports Explained (Very Important)

There are **2 ways** to expose ports in Docker:

---

# Option 1 — Dynamic Ports (Recommended for Multiple Projects)

Example:

```yaml
ports:
  - "80"
```

or

```yaml
ports:
  - "8025"
```

This means:

Docker automatically assigns a free host port.

Example:

```bash
0.0.0.0:49153->80/tcp
```

This is AMAZING when running:

* multiple Laravel projects
* multiple Docker stacks
* many local development environments

because you avoid port conflicts.

## Example Use Case

You have:

* Project A
* Project B
* Project C

All using Nginx + phpMyAdmin

Dynamic ports allow all of them to run together without:

```text
Port already in use
```

errors.

## Check Assigned Ports

Run:

```bash
docker compose ps
```

Docker will show actual ports.

Example:

```bash
0.0.0.0:49153->80/tcp
0.0.0.0:49154->80/tcp
0.0.0.0:49155->8025/tcp
```

---

# Option 2 — Fixed Ports (Recommended for Single Project)

Example:

```yaml
ports:
  - "8000:80"
```

This means:

```text
localhost:8000 → container:80
```

This is BEST when:

* you work on only one project
* you want stable URLs
* you prefer convenience

No need to check assigned ports.

Just open:

```text
http://localhost:8000
```

every time.

---

# ✅ Recommended Fixed Ports Setup

If you want stable URLs, use:

```yaml
nginx:
  ports:
    - "8000:80"

mailhog:
  ports:
    - "8025:8025"

phpmyadmin:
  ports:
    - "8080:80"
```

This gives:

| Service     | URL                   |
| ----------- | --------------------- |
| Laravel App | http://localhost:8000 |
| phpMyAdmin  | http://localhost:8080 |
| Mailhog     | http://localhost:8025 |

This is the most beginner-friendly setup.

---

# 🔥 Which Should You Use?

## Use Dynamic Ports If:

* you run multiple projects
* you work with many Docker environments
* you want zero port conflicts

## Use Fixed Ports If:

* you run one main project
* you want easy URLs
* you prefer simplicity

---

# 🧱 Project Structure

```text
.
├── docker-compose.yml
├── Dockerfile
├── docker-entrypoint.sh
├── .env.docker
├── nginx/
│   └── default.conf
└── ...
```

---

# ⚙️ Environment Configuration

This project uses:

```text
.env.docker
```

which is automatically copied to:

```text
.env
```

during container startup.

You can customize:

* database credentials
* Redis config
* mail settings
* app URL
* queue settings
* session settings

by editing:

```text
.env.docker
```

---

# 🛢️ Database Configuration

## MySQL Service

### Container Name

```text
mysql
```

### Default Credentials

| Key           | Value      |
| ------------- | ---------- |
| Host          | mysql      |
| Port          | 3306       |
| Database      | laravel_db |
| Username      | user       |
| Password      | user123    |
| Root Password | root       |

Laravel uses this automatically.

---

# 🔴 Redis Configuration

Redis is used for:

* Cache
* Sessions
* Queues
* Background jobs

## Redis Container

```text
redis
```

## Redis Port

```text
6379
```

---

# 📬 Mailhog Configuration

Mailhog captures all outgoing emails locally.

No real emails are sent.

Perfect for testing emails safely.

## SMTP Settings

| Key      | Value   |
| -------- | ------- |
| Host     | mailhog |
| Port     | 1025    |
| Username | null    |
| Password | null    |

## Mailhog UI

```text
http://localhost:8025
```

---

# 🛠 Useful Commands

---

# View All Logs

```bash
docker compose logs -f
```

---

# View Specific Service Logs

```bash
docker compose logs -f app
docker compose logs -f nginx
docker compose logs -f mysql
docker compose logs -f redis
```

---

# Access App Container

```bash
docker compose exec app bash
```

---

# Run Artisan Commands

```bash
docker compose exec app php artisan migrate
docker compose exec app php artisan config:clear
docker compose exec app php artisan cache:clear
docker compose exec app php artisan route:clear
```

---

# Stop Containers

```bash
docker compose down
```

---

# Remove Everything (Including Database)

```bash
docker compose down -v
```

⚠️ This deletes MySQL data.

Use carefully.

---

# Rebuild Containers

```bash
docker compose up -d --build
```

Use this when:

* Dockerfile changes
* PHP extensions change
* major Docker config changes happen

---

# 🧪 Development Notes

* Code changes reflect instantly via volume mounts
* No rebuild needed for Laravel/PHP changes
* Rebuild only when Docker-level configuration changes

This makes development extremely fast.

---

# ⚠️ Important Notes

* This setup is for **development only**
* Never use default credentials in production
* Ports are exposed for convenience
* Xdebug is installed but disabled by default

To enable Xdebug:

```ini
xdebug.mode=debug
```

inside:

```text
/usr/local/etc/php/conf.d/xdebug.ini
```

---

# 🧠 Service Breakdown

---

# app

Runs:

* PHP 8.4 FPM
* Laravel
* Composer
* Artisan commands

This is your main application container.

---

# nginx

Handles:

* HTTP requests
* Static file serving
* PHP request forwarding

Acts as the frontend web server.

---

# mysql

Persistent MySQL database using:

```text
mysql_data
```

Your data survives rebuilds.

---

# redis

Handles:

* sessions
* cache
* queues

Improves Laravel performance.

---

# mailhog

Captures local emails.

Perfect for development.

---

# phpmyadmin

Database GUI for MySQL.

Useful for:

* viewing tables
* running SQL queries
* imports / exports
* debugging database issues

---

# 🚀 Future Improvements

Potential upgrades:

* Laravel Horizon
* Laravel Telescope
* Supervisor workers
* Queue dashboard
* HTTPS support
* Production-ready deployment
* CI/CD integration

---

# 🙌 Contribution

Feel free to:

* open issues
* submit pull requests
* improve the starter kit

Contributions are welcome.

---

# 📜 License

Open-source and free to use.

Use it however you like.

---

# 💡 Quick Start Summary

```bash
docker compose up -d --build
```

You get:

* Laravel
* MySQL
* Redis
* Nginx
* Mailhog
* phpMyAdmin

fully working in minutes.

A complete Laravel Docker development environment 🚀
