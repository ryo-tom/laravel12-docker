FROM php:8.2-fpm

WORKDIR /var/www/html

RUN set -eux; \
  apt-get update || (sleep 5 && apt-get update); \
  apt-get install -y --no-install-recommends \
    git unzip ca-certificates curl \
    libzip-dev \
    libpng-dev libjpeg62-turbo-dev libfreetype6-dev \
    libicu-dev \
    default-mysql-client; \
  docker-php-ext-configure gd --with-freetype --with-jpeg; \
  docker-php-ext-install -j"$(nproc)" pdo_mysql gd zip bcmath intl opcache; \
  apt-get clean; \
  rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Composer
COPY --from=composer:2 /usr/bin/composer /usr/local/bin/composer

# Node.js
ARG NODE_MAJOR=22
RUN curl -fsSL https://deb.nodesource.com/setup_${NODE_MAJOR}.x | bash - \
 && apt-get install -y nodejs \
 && npm install -g npm@latest \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*


# npm / Composer caches (avoid EACCES)
ENV COMPOSER_HOME=/var/www/html/.composer \
    NPM_CONFIG_CACHE=/var/www/html/.npm
RUN install -d -m 0775 -o www-data -g www-data "$COMPOSER_HOME" "$NPM_CONFIG_CACHE"

USER www-data

EXPOSE 9000 5173
CMD ["php-fpm"]
