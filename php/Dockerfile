FROM php:8.1-fpm-alpine

WORKDIR "/var/www/html"

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN echo "UPDATING DEPENDENCIES..."

RUN apk update && apk add \
	mc \
	nano \
    build-base \
    freetype-dev \
    libjpeg-turbo-dev \
    libpng-dev \
    libwebp-dev \
    libzip-dev \
    zip \
    unzip \
    git \
    oniguruma-dev \
    curl \
    libmcrypt-dev \
    neovim \
    bash \
    openssh-client \
    postgresql-dev

## Install PDO-MySQL Extension
#RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl

# Install PDO-PostgreSQL Extension
RUN docker-php-ext-install pdo_pgsql mbstring zip exif pcntl

RUN apk add --update linux-headers

# Install Redis Extension
RUN echo "INSTALLING REDIS..."
RUN apk add autoconf && pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis && apk del autoconf

# Install xDebug
#RUN echo "INSTALLING XDEBUG..."
#RUN apk add --no-cache $PHPIZE_DEPS \
#    && pecl install xdebug \
#    && docker-php-ext-enable xdebug

# Get latest Composer
RUN echo "INSTALLING COMPOSER..."
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Images formats
RUN docker-php-ext-configure gd --with-freetype --with-webp --with-jpeg && \
  docker-php-ext-install gd

# Install Node
RUN echo "INSTALLING NODE..."
RUN apk add nodejs npm
