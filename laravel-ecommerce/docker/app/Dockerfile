FROM php:8.4.11-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    nodejs \
    npm

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www

# Copy application files (يجب إضافة هذا السطر)
COPY --chown=developer:developer . .

# Create non-root user
RUN useradd -ms /bin/bash -u 1337 developer && chown -R developer:developer /var/www
USER developer

# Install PHP dependencies (يجب إضافة هذا السطر)
RUN composer install --no-dev --optimize-autoloader

# Add Laravel CLI tool
RUN composer global require laravel/installer

# Add global composer bin to PATH
ENV PATH="/var/www/vendor/bin:/home/developer/.composer/vendor/bin:${PATH}"

EXPOSE 9000

CMD ["php-fpm"]
