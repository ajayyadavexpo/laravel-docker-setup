# 🚀 Laravel Docker Setup (PHP 8.4 + Nginx + MySQL + Redis + Queue + Mailhog)

A complete **production-like Dockerized Laravel development environment** with:

* ⚡ PHP 8.4 (FPM)
* 🌐 Nginx
* 🛢️ MySQL 8
* 🔴 Redis (Cache, Queue, Sessions)
* 📬 Mailhog (Email testing)
* ⚙️ Queue Worker & Scheduler
* 🧰 phpMyAdmin
* 📦 Composer

---

# 📦 Prerequisites

Make sure you have:

* Docker
* Docker Compose (v2+)
* Git

Check versions:

```bash
docker -v
docker compose version
```

---

# 📁 Project Structure

```
.
├── Dockerfile
├── docker-compose.yml
├── docker-entrypoint.sh
├── nginx/
│   └── default.conf
├── .env
├── app/
├── routes/
├── vendor/
├── node_modules/
└── ...
```

---

# ⚡ Quick Start (TL;DR)

```bash
git clone <your-repo-url>
cd <project>

cp .env.example .env

docker compose up -d --build

docker compose exec app php artisan key:generate
docker compose exec app php artisan migrate
```

👉 Open: [http://localhost:8000](http://localhost:8000)

---

# 🧱 Services Overview

| Service      | Description            |
| ------------ | ---------------------- |
| `app`        | Laravel (PHP-FPM)      |
| `nginx`      | Web server             |
| `mysql`      | Database               |
| `redis`      | Cache, queue, sessions |
| `queue`      | Laravel queue worker   |
| `scheduler`  | Laravel cron scheduler |
| `mailhog`    | Email testing          |
| `phpmyadmin` | Database UI            |

---

# ⚙️ Setup Instructions

## 1. Clone Repository

```bash
git clone <your-repo-url>
cd <project>
```

---

## 2. Install Laravel (if not present)

```bash
docker run --rm -v ${PWD}:/app composer create-project laravel/laravel .
```

---

## 3. Setup Environment

```bash
cp .env.example .env
```

### 🔧 Update `.env`

```env
APP_NAME=Laravel
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost:8000

# Database
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel_db
DB_USERNAME=user
DB_PASSWORD=user123

# Redis
REDIS_HOST=redis

# Queue
QUEUE_CONNECTION=redis

# Session & Cache
SESSION_DRIVER=redis
CACHE_STORE=redis

# Mailhog
MAIL_MAILER=smtp
MAIL_HOST=mailhog
MAIL_PORT=1025
```

---

## 4. Build & Start

```bash
docker compose up -d --build
```

---

## 5. Install Dependencies

```bash
docker compose exec app composer install
```

---

## 6. Generate App Key ⚠️ IMPORTANT

```bash
docker compose exec app php artisan key:generate
```

---

## 7. Run Migrations

```bash
docker compose exec app php artisan migrate
```

---

# 🌐 Access URLs

| Service     | URL                                            |
| ----------- | ---------------------------------------------- |
| Laravel App | [http://localhost:8000](http://localhost:8000) |
| phpMyAdmin  | [http://localhost:8080](http://localhost:8080) |
| Mailhog UI  | [http://localhost:8025](http://localhost:8025) |

---

# 🧪 Health Check Endpoint (Optional but Recommended)

Add this to `routes/web.php`:

```php
Route::get('/', function () {
    return response()->json([
        'app' => 'Laravel running 🚀',
        'time' => now()
    ]);
});
```

👉 Visit: [http://localhost:8000](http://localhost:8000)

---

# 🔍 Verify Services

Run inside container:

```bash
docker compose exec app php artisan tinker
```

```php
DB::connection()->getPdo();   // MySQL
Redis::ping();                // Redis
```

---

# 🛢️ Database Access

| Key      | Value      |
| -------- | ---------- |
| Host     | mysql      |
| Port     | 3306       |
| DB       | laravel_db |
| User     | user       |
| Password | user123    |

---

# ⚙️ Queue & Scheduler

### Queue Worker (auto-running)

```bash
docker compose logs -f queue
```

### Scheduler (cron simulation)

```bash
docker compose logs -f scheduler
```

---

# 📬 Email Testing (Mailhog)

* UI: [http://localhost:8025](http://localhost:8025)
* All emails are captured here (no real emails sent)

---

# 🔧 Useful Commands

## Logs

```bash
docker compose logs -f
```

## Access Container

```bash
docker compose exec app bash
```

## Artisan

```bash
docker compose exec app php artisan <command>
```

## Restart

```bash
docker compose restart
```

## Stop

```bash
docker compose down
```

## Reset (⚠️ Deletes DB)

```bash
docker compose down -v
```

---

# 🧠 How It Works

### 🐘 PHP (app)

* Runs Laravel via PHP-FPM
* Handles business logic

### 🌐 Nginx

* Serves `/public`
* Connects to PHP via port 9000

### 🛢️ MySQL

* Stores data
* Persistent volume

### 🔴 Redis

* Cache
* Sessions
* Queue backend

### ⚙️ Queue

* Runs background jobs

### ⏰ Scheduler

* Runs Laravel cron tasks

### 📬 Mailhog

* Captures emails locally

---

# 🔐 Permissions (Auto-handled)

```bash
storage/
bootstrap/cache/
```

* Owned by `www-data`
* Permissions set to `775`

---

# ⚠️ Common Issues & Fixes

## ❌ APP_KEY Missing

```bash
docker compose exec app php artisan key:generate
```

---

## ❌ Database Not Connecting

* Check `.env`
* Ensure MySQL is healthy

```bash
docker compose logs mysql
```

---

## ❌ Redis Not Working

```bash
docker compose logs redis
```

---

## ❌ Blank Page / 500 Error

```bash
docker compose logs app
```

---

## ❌ Permission Issues

```bash
docker compose exec app chmod -R 775 storage bootstrap/cache
```

---

# 🔁 Rebuild Containers

```bash
docker compose up -d --build
```

---

# 📦 Volumes

| Volume     | Purpose        |
| ---------- | -------------- |
| mysql_data | DB persistence |

---

# 🧪 Development Tips

* ✅ Live reload (via volume mount)
* ❌ No rebuild needed for code changes
* 🔁 Rebuild only if:

  * Dockerfile changes
  * PHP extensions added

---

# 🛑 Security Notes

⚠️ Development only

* Do NOT use default passwords in production
* Remove exposed ports in production
* Use secrets manager in real deployments

---

# 🚀 Next Improvements (Optional)

* Add **Xdebug for debugging**
* Add **Laravel Horizon (queue UI)**
* Add **Elasticsearch / Meilisearch**
* Add **CI/CD pipeline**

---

# 🙌 Contribution

PRs are welcome!

---

# 📜 License

Open-source and free to use.

---

# 💡 Final Summary

Run this:

```bash
docker compose up -d --build
```

👉 And you get a **full Laravel production-like environment locally** — including queue, cache, scheduler, and email testing.

