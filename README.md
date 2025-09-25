# Docker Laravel

A Docker development environment for Laravel applications with PHP 8.2, MySQL, and Nginx.

## 📋 Requirements

- Docker 20.x or higher
- Docker Compose 2.x or higher

## 🏗️ Project Structure

```bash
docker-laravel/
├── docker/
│   ├── mysql/
│   │   └── my.cnf              # MySQL configuration
│   ├── nginx/
│   │   └── default.conf        # Nginx virtual host
│   └── php/
│       ├── Dockerfile          # PHP-FPM image
│       └── php.ini             # PHP configuration
├── src/                        # Laravel application root
│   └── ...
├── .dockerignore
├── .gitignore
├── docker-compose.yml
└── README.md
```

## 🚀 Tech Stack

- **PHP**: 8.2-FPM
- **Composer**: 2.x (latest)
- **Laravel**: 12.x
- **MySQL**: 8.0
- **Nginx**: Latest
- **Node.js**: 22.x LTS
- **npm**: Latest

## 🛠️ Quick Start

### New Laravel Project

```bash
# Start containers
docker compose up -d --build

# Enter container
docker compose exec app bash
```

Inside container:

The working directory is already `/var/www/html/src` (empty on first run). 

```bash
composer create-project laravel/laravel . "12.*" --prefer-dist
```

> Note: We don’t use `laravel new` here because it only runs in a completely empty target directory and relies on a writable Composer global home. In a Docker setup running as a non-root user, that adds fragile permission/setup steps and often breaks. Using composer create-project is simpler and more reproducible.

### Setup Environment & Database

Setup environment:

```bash
cp .env.example .env
php artisan key:generate
```

Update `.env` file:

```env
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel_db
DB_USERNAME=laravel_user
DB_PASSWORD=laravel_password
```

Run migrations:

```bash
php artisan migrate
```

### Vite HMR in Docker

Add this to `vite.config.ts` so the dev server binds inside the container and the browser connects from the host:

```ts
server: {
    host: true,
    hmr: {
        host: 'localhost',
    },
},
```

## 🌐 Access URLs

- **Application**: <http://localhost>

## 💡 Tips

Common Commands:

```bash
# Container management
docker compose up -d          # Start containers
docker compose down           # Stop containers
docker compose logs -f app    # View logs

# Enter container for multiple commands
docker compose exec app bash

# Laravel commands (inside container)
php artisan migrate
php artisan make:controller
php artisan tinker

# Frontend commands (inside container)
npm run dev
npm run build
npm run watch

# Or run single commands from outside
docker compose exec app php artisan migrate
docker compose exec app composer install
```
