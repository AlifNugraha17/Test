FROM php:8.2-cli

# Install dependensi PostgreSQL, SQLite & ekstensi PHP
RUN apt-get update && apt-get install -y \
    git unzip libpq-dev libsqlite3-dev \
    && docker-php-ext-install pdo pdo_pgsql pgsql pdo_sqlite

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www

COPY . .

# Install paket composer
RUN composer install --no-dev --optimize-autoloader

# Atur hak akses folder storage & cache
RUN chown -R www-data:www-data storage bootstrap/cache

# Jalankan migrasi database otomatis & server Laravel saat menyala
CMD sh -c "php artisan migrate --force && php artisan db:seed --class=TourismSeeder --force && php artisan serve --host 0.0.0.0 --port ${PORT:-8000}"
