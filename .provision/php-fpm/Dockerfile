# Based on https://github.com/Cyber-Duck/php-fpm-laravel/tree/7.2
FROM php:7.4-fpm

ENV XDEBUG="false"

RUN apt-get update && \
    apt-get install -y --force-yes --no-install-recommends \
        libmemcached-dev \
        libz-dev \
        libpq-dev \
        libjpeg-dev \
        libpng-dev \
        libfreetype6-dev \
        libssl-dev \
        openssh-server \
        libmagickwand-dev \
        git \
        cron \
        nano \
        libxml2-dev \
        libzip-dev \
        zip \
        unzip

RUN docker-php-ext-install \
        soap \
        pcntl \
        pdo_mysql \
        pdo_pgsql \
        bcmath \
        zip

RUN pecl install imagick && \
    docker-php-ext-enable imagick

RUN docker-php-ext-configure gd \
        --with-freetype \
        --with-jpeg && \
    docker-php-ext-install gd

RUN docker-php-ext-install exif && \
    docker-php-ext-configure exif \
        --enable-exif

# Install the xdebug extension
RUN pecl install xdebug && docker-php-ext-enable xdebug
# Copy xdebug configration for remote debugging
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# Install the php memcached extension
RUN pecl install memcached && docker-php-ext-enable memcached

# Install composer and add its bin to the PATH.
RUN curl -s http://getcomposer.org/installer | php && \
    echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc && \
    mv composer.phar /usr/local/bin/composer

# Source the bash
RUN . ~/.bashrc


ADD ./php_fpm.ini /usr/local/etc/php/conf.d

RUN rm -r /var/lib/apt/lists/*

RUN usermod -u 1000 www-data

WORKDIR /var/www

COPY ./docker-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-entrypoint.sh
RUN ln -s /usr/local/bin/docker-entrypoint.sh /
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 9000
CMD ["php-fpm"]
