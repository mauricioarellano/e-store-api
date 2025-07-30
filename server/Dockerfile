FROM php:8.2-apache

# Install dependencies
RUN apt-get update && apt-get install -y \
    git curl libpng-dev libonig-dev libxml2-dev zip unzip \
    && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Enable Apache mod_rewrite
RUN a2enmod rewrite

# Set working directory
WORKDIR /app

# Copy project files
COPY . /app/

# Install Composer dependencies
RUN cd /app && composer install --no-dev --optimize-autoloader

# Copy to Apache directory and set permissions
RUN cp -r /app/* /var/www/html/ && \
    chown -R www-data:www-data /var/www/html && \
    chmod -R 755 /var/www/html

# Copy Apache config
COPY .conf/apache.conf /etc/apache2/sites-available/000-default.conf

EXPOSE 80
CMD ["apache2-foreground"]
