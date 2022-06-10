FROM php:7.4-fpm

## Install composer
RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt-get update && apt-get install -y \
    git \
    vim \
    tzdata \
    zip unzip \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install -j$(nproc) gd

# https://matthewsetter.com/setup-step-debugging-php-xdebug3-docker/
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

WORKDIR /var/www

# Expose port 9000 and start php-fpm server
EXPOSE 9000

CMD ["/usr/local/sbin/php-fpm"]
