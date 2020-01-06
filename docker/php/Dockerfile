FROM php:7.2-fpm

# Install required librairies
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    zlib1g-dev \
    libjpeg-dev\
    libpng-dev\
    libfreetype6-dev \
    libpq-dev \
    libicu-dev g++

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Configure PHP extensions
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/

# Install PHP extensions
RUN docker-php-ext-install pdo pdo_mysql gd mysqli zip gd intl

# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
RUN { \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=2'; \
		echo 'opcache.fast_shutdown=1'; \
		echo 'opcache.enable_cli=1'; \
} > /usr/local/etc/php/conf.d/opcache-recommended.ini

# Import custom php.ini
COPY ./php.ini /usr/local/etc/php/

WORKDIR /var/www
