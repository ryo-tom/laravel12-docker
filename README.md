# Docker Laravel

A Docker development environment for Laravel applications with PHP 8.2, MySQL, and Nginx.

## 📋 Prerequisites

- Docker 20.x or higher
- Docker Compose 2.x or higher

## 🏗️ Project Structure

```bash
/
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

|Service|Version|Purpose|
|---|---|---|
|PHP-FPM|8.2|Application runtime|
|Composer|2.x|PHP dependency management|
|Laravel|12.x|Web application framework|
|MySQL|8.0|Primary database|
|Nginx|latest|Web server & reverse proxy|
|Node.js|22.x LTS|Frontend tooling|

## 🛠️ Getting Started

Start the containers:

```bash
docker compose up -d --build
```

## 📦 Installing Laravel

 Enter the application container: 

```bash
docker compose exec app bash
```

The container’s working directory is already set to `/var/www/html/src`. On the first run this directory will be empty, and you can install Laravel inside it.

You have two options to bootstrap a new Laravel project:

### Option 1: Using Composer

```bash
# Install Laravel into the current directory (src/)
composer create-project laravel/laravel . "12.*" --prefer-dist
```

### Option 2: Using Laravel Installer (for starter kits)

If you need Laravel’s official starter kits, use the Laravel Installer.

```bash
# Install Laravel installer globally
composer global require laravel/installer

# Add to PATH
export PATH="$PATH:/var/www/html/.composer/vendor/bin"

# Move to parent directory
cd ..

# Install with force flag to use existing src/ directory
laravel new -f src
```

## ⚙️ Configure Environment

1. Copy the example environment file:

```bash
cp .env.example .env
```

2. Generate the app key:

```bash
php artisan key:generate
```

3. Update `.env` with database settings:

```env
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel_db
DB_USERNAME=laravel_user
DB_PASSWORD=laravel_password
```

4. Run migrations:

```bash
php artisan migrate
```

## 🔥 Frontend (Vite)

Laravel uses Vite for asset bundling.

1. Update `vite.config.ts` so the dev server binds inside Docker and connects from the host:

```ts
server: {
  host: true,
  hmr: { host: 'localhost' },
},
```

2. Start the Vite dev server for hot reload:

```bash
npm install
npm run dev
```

Now changes to JS/CSS will update immediately at [http://localhost](http://localhost).

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
