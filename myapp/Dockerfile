FROM php:8.2-fpm

# Install dependencies
RUN apt-get update && apt-get install -y \
    git unzip libzip-dev curl && \
    docker-php-ext-install zip pdo pdo_mysql

# Install Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www

# Copy source code
COPY . .

# Generate .env jika belum ada
RUN if [ ! -f .env ]; then cp .env.example .env; fi

# Install Laravel dependencies
RUN composer install --no-dev --optimize-autoloader

# Generate Laravel APP_KEY
RUN php artisan key:generate

# Set permissions
RUN chown -R www-data:www-data /var/www/storage /var/www/bootstrap/cache

EXPOSE 9000
CMD ["php-fpm"]
