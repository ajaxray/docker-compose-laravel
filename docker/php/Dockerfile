FROM php:7.2-fpm

# Set working directory
WORKDIR /var/www

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    mariadb-client \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    libmagickwand-dev --no-install-recommends

# PHP Extensions
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl bcmath

# ImageMagick - I prefer ImageMagick and ghostscript instead of gd.
# If your choice is different, you may comment out the following section and uncomment GD section
RUN apt-get install ghostscript -y \
    && pecl install imagick \
    && docker-php-ext-enable imagick

# GD - Uncomment next 2 lines to enable gd image processing library
# RUN docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/
# RUN docker-php-ext-install gd

# composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Nodejs - nodejs and npm
# It's required for VueJS and Laravel-Mix
RUN apt-get install gnupg -yq \
    && curl -sL https://deb.nodesource.com/setup_8.x | bash \
    && apt-get install nodejs -yq \
    && curl -L https://npmjs.org/install.sh | bash

# Clear apt cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory contents
COPY . /var/www

# Copy existing application directory permissions
COPY --chown=www:www . /var/www

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
