# 🚀 Laravel Docker Starter Kit

A **zero-setup Laravel development environment** using Docker.

Just clone and run — Laravel installs automatically with MySQL, Redis, Nginx, Mailhog, and phpMyAdmin ready out of the box.

Perfect for fast local Laravel development with **PHP 8.4 + MySQL 8 + Redis + Nginx**.

---

# ⚡ Features

* PHP 8.4 (FPM)
* Nginx (fast web server)
* MySQL 8 (database)
* Redis (cache, queue, sessions)
* Mailhog (email testing)
* phpMyAdmin (database UI)
* Xdebug (installed, disabled by default)
* Auto Laravel installation on first run
* Auto `.env` configuration
* Auto database migration
* Auto permission fixing

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

No manual artisan commands required.

---

# 📦 Docker Services

This setup includes the following containers:

| Service      | Container Purpose             |
| ------------ | ----------------------------- |
| `app`        | PHP-FPM + Laravel application |
| `nginx`      | Web server                    |
| `mysql`      | MySQL database                |
| `redis`      | Redis cache / queue / session |
| `mailhog`    | Local email testing           |
| `phpmyadmin` | Database management UI        |

---

# 🌐 Exposed Ports

## Important

Your current `docker-compose.yml` uses **dynamic ports** for `nginx`, `mailhog`, and `phpMyAdmin`.

That means Docker automatically assigns available ports instead of fixed ones.

### Current Port Configuration

```yaml
nginx:
  ports:
    - "80"

mailhog:
  ports:
    - "8025"

phpmyadmin:
  ports:
    - "80"
```

This means:

* Laravel app → random host port mapped to container port `80`
* phpMyAdmin → random host port mapped to container port `80`
* Mailhog → random host port mapped to container port `8025`

To check the actual assigned ports:

```bash
docker compose ps
```

Example output:

```bash
0.0.0.0:49153->80/tcp
0.0.0.0:49154->80/tcp
0.0.0.0:49155->8025/tcp
```

---

# ✅ Recommended Fixed Ports (Best Practice)

For easier local development, update your ports like this:

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

This gives you stable URLs:

| Service     | URL                   |
| ----------- | --------------------- |
| Laravel App | http://localhost:8000 |
| phpMyAdmin  | http://localhost:8080 |
| Mailhog     | http://localhost:8025 |

Highly recommended.

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
* Redis settings
* mail configuration
* Laravel app URL

by editing:

```text
.env.docker
```

---

# 🛢️ Database Configuration

## MySQL Container

### Service Name

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

Used automatically by Laravel.

---

# 🔴 Redis Configuration

Redis is used for:

* Cache
* Sessions
* Queue
* Laravel jobs

### Redis Host

```text
redis
```

### Redis Port

```text
6379
```

---

# 📬 Mailhog Configuration

Mailhog captures all outgoing emails locally.

## SMTP Settings

| Key      | Value   |
| -------- | ------- |
| Host     | mailhog |
| Port     | 1025    |
| Username | null    |
| Password | null    |

## Web UI

```text
http://localhost:8025
```

No real emails are sent.

Perfect for development.

---

# 🛠 Useful Commands

---

## View all logs

```bash
docker compose logs -f
```

---

## View specific service logs

```bash
docker compose logs -f app
docker compose logs -f nginx
docker compose logs -f mysql
```

---

## Access Laravel app container

```bash
docker compose exec app bash
```

---

## Run Artisan Commands

```bash
docker compose exec app php artisan migrate
docker compose exec app php artisan cache:clear
docker compose exec app php artisan config:clear
```

---

## Stop all containers

```bash
docker compose down
```

---

## Remove everything including database volume

```bash
docker compose down -v
```

---

## Rebuild containers

```bash
docker compose up -d --build
```

---

# 🧪 Development Notes

* Code changes reflect instantly via Docker volumes
* No rebuild needed for Laravel/PHP file changes
* Rebuild only if:

  * Dockerfile changes
  * PHP extensions change
  * Nginx config changes

Fast local development workflow.

---

# ⚠️ Important Notes

* This setup is for **development only**
* Never use default credentials in production
* Ports are exposed for local convenience
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

## app

Runs:

* PHP 8.4 FPM
* Laravel
* Composer
* Artisan commands

This is your main application container.

---

## nginx

Handles:

* HTTP requests
* Static files
* PHP forwarding to FPM

Acts as the frontend web server.

---

## mysql

Persistent MySQL database with Docker volume:

```text
mysql_data
```

Your data survives container rebuilds.

---

## redis

Handles:

* caching
* queues
* sessions

Improves Laravel performance.

---

## mailhog

Captures emails for local development.

Safe testing without sending real emails.

---

## phpmyadmin

Provides a GUI for MySQL management.

Useful for:

* viewing tables
* importing/exporting SQL
* debugging database issues

---

# 🚀 Future Improvements

Possible upgrades:

* Laravel Horizon
* Laravel Telescope
* Supervisor for workers
* HTTPS support
* Production-ready deployment setup
* CI/CD pipeline integration

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
