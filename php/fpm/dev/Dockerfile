FROM php:7.3-fpm-alpine

ENV PHPIZE_DEPS \
		autoconf \
		dpkg-dev dpkg \
		file \
		g++ \
		gcc \
		libc-dev \
		make \
		pkgconf \
    re2c \
    freetype-dev \
    libpng-dev \
    libjpeg-turbo-dev \
    zlib-dev \
    libzip-dev

RUN true && \
  apk add --no-cache $PHPIZE_DEPS freetype libpng libjpeg-turbo && \
  docker-php-ext-configure gd \
    --with-gd \
    --with-freetype-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ && \
  NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
  docker-php-ext-install -j${NPROC} gd pdo_mysql zip && \
  apk add --no-cache --virtual .build-deps $PHPIZE_DEPS && \
  pecl install redis && \
  docker-php-ext-enable redis && \
  apk del --no-network .build-deps