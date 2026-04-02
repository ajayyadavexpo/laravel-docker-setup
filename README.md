# 🚀 Laravel Docker Starter Kit

A **zero-setup Laravel development environment** using Docker.

Just clone and run — Laravel installs automatically with MySQL, Redis, Nginx, Queue, Scheduler, and Mailhog ready out of the box.

---

## ⚡ Features

* PHP 8.4 (FPM)
* Nginx (fast web server)
* MySQL 8 (database)
* Redis (cache, queue, sessions)
* Queue worker (auto running)
* Scheduler (cron simulation)
* Mailhog (email testing)
* phpMyAdmin (database UI)
* Xdebug (installed, disabled by default)

---

## 🎯 Goal

No manual setup required.

```bash
git clone <your-repo>
cd <project>
docker compose up -d --build
```

That’s it.

---

## 🧠 What Happens Automatically

On first run:

* Laravel is installed (if not present)
* `.env` is configured using `.env.docker`
* Dependencies are installed
* Application key is generated
* Database connection is verified
* Migrations are executed
* Permissions are fixed

---

## 🌐 Access URLs

| Service    | URL                   |
| ---------- | --------------------- |
| Laravel    | http://localhost:8000 |
| phpMyAdmin | http://localhost:8080 |
| Mailhog    | http://localhost:8025 |

---

## 🧱 Project Structure

```
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

## ⚙️ Environment Configuration

This project uses:

```
.env.docker
```

It is automatically copied to:

```
.env
```

on container startup.

You can modify `.env.docker` to customize:

* Database credentials
* Redis config
* Mail settings
* App URL

---

## 🛢️ Database Details

| Key      | Value      |
| -------- | ---------- |
| Host     | mysql      |
| Port     | 3306       |
| DB       | laravel_db |
| User     | user       |
| Password | user123    |

---

## 🔴 Redis

Used for:

* Cache
* Sessions
* Queue

---

## ⚙️ Queue & Scheduler

These run automatically in separate containers:

```bash
docker compose logs -f queue
docker compose logs -f scheduler
```

---

## 📬 Mailhog (Email Testing)

* SMTP: `mailhog:1025`
* Web UI: http://localhost:8025

All emails are captured here.

---

## 🔧 Useful Commands

### View logs

```bash
docker compose logs -f
```

### Access app container

```bash
docker compose exec app bash
```

### Run artisan

```bash
docker compose exec app php artisan <command>
```

### Stop containers

```bash
docker compose down
```

### Reset everything (including DB)

```bash
docker compose down -v
```

---

## 🔁 Rebuild Containers

```bash
docker compose up -d --build
```

---

## 🧪 Development Notes

* Code changes reflect instantly (volume mount)
* No rebuild needed for PHP/Laravel changes
* Rebuild only if:

  * Dockerfile changes
  * PHP extensions change

---

## ⚠️ Important Notes

* This setup is intended for **development only**
* Default credentials should NOT be used in production
* Ports are exposed for convenience

---

## 🧠 How It Works

### app (PHP-FPM)

Runs Laravel and handles backend logic.

### nginx

Serves the application and forwards PHP requests.

### mysql

Persistent database with Docker volume.

### redis

Handles cache, sessions, and queues.

### queue

Processes background jobs.

### scheduler

Runs scheduled Laravel tasks.

### mailhog

Captures emails locally.

### phpmyadmin

Provides database UI.

---

## 🚀 Future Improvements

* Laravel Horizon (queue dashboard)
* Laravel Telescope (debugging)
* HTTPS support
* Production-ready configuration

---

## 🙌 Contribution

Feel free to open issues or submit pull requests.

---

## 📜 License

Open-source and free to use.

---

## 💡 Summary

```bash
docker compose up -d --build
```

You get a **fully working Laravel environment instantly**.
