FROM php:8.2-cli

# 1. Install ekstensi PostgreSQL & SQLite untuk PHP
RUN apt-get update && apt-get install -y \
    git unzip libpq-dev libsqlite3-dev \
    && docker-php-ext-install pdo pdo_pgsql pgsql pdo_sqlite

# 2. Install Composer ke dalam container
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www

COPY . .

# 3. Install semua dependensi Laravel
RUN composer install --no-dev --optimize-autoloader

# 4. Buat folder storage & cache jika belum ada & atur izin 777
RUN mkdir -p storage/framework/cache storage/framework/sessions storage/framework/views bootstrap/cache database \
    && chmod -R 777 storage bootstrap/cache database

# 5. Jalankan migrasi database otomatis & nyalakan server Laravel
CMD sh -c "touch database/database.sqlite && php artisan migrate --force && php artisan serve --host 0.0.0.0 --port ${PORT:-8000}"