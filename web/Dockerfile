FROM php:8.3-apache

# Install necessary packages and PHP extensions
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    git \
    curl \
    gnupg \
    build-essential \
    libssl-dev \
    zlib1g-dev \
    libbz2-dev \
    libreadline-dev \
    libsqlite3-dev \
    tzdata

RUN docker-php-ext-install pdo pdo_mysql mbstring exif pcntl bcmath gd

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Install nodenv
RUN git clone https://github.com/nodenv/nodenv.git /usr/local/nodenv \
    && cd /usr/local/nodenv && src/configure && make -C src \
    && echo 'export PATH="/usr/local/nodenv/bin:$PATH"' >> /root/.bashrc \
    && echo 'eval "$(nodenv init -)"' >> /root/.bashrc

# Install node-build plugin for nodenv
RUN mkdir -p /usr/local/nodenv/plugins/node-build \
    && git clone https://github.com/nodenv/node-build.git /usr/local/nodenv/plugins/node-build \
    && echo 'export PATH="/usr/local/nodenv/plugins/node-build/bin:$PATH"' >> /root/.bashrc \
    && /usr/local/nodenv/bin/nodenv rehash

# Set environment variables for Apache
ENV APACHE_DOCUMENT_ROOT /var/www/html/public

# Enable Apache mod_rewrite
RUN a2enmod rewrite

# Set the timezone to JST
ENV TZ=Asia/Tokyo

# Copy the virtual host configuration file
COPY ./org-apache-config.conf /etc/apache2/sites-available/000-default.conf

# Copy php.ini configuration
COPY php/php.ini /usr/local/etc/php/

EXPOSE 80
CMD ["apache2-foreground"]
