FROM php:8.2-cli

# 1. Install ekstensi PostgreSQL untuk PHP
RUN apt-get update && apt-get install -y \
    git unzip libpq-dev \
    && docker-php-ext-install pdo pdo_pgsql pgsql

# 2. Install Composer ke dalam container
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www

COPY . .

# 3. Install semua dependensi Laravel
RUN composer install --no-dev --optimize-autoloader

# 4. Beri izin akses folder storage & cache
RUN chown -R www-data:www-data storage bootstrap/cache

# 5. Jalankan migrasi database otomatis & nyalakan server Laravel
CMD sh -c "php artisan migrate --force && php artisan serve --host 0.0.0.0 --port ${PORT:-8000}"