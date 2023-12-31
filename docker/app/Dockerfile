FROM php:8.1-fpm

# php.iniをコピー
COPY ./docker/app/php.ini /usr/local/etc/app/php.ini

# Composer install
COPY --from=composer:2.5 /usr/bin/composer /usr/bin/composer

# node.js install(reactを使用するため)
COPY --from=node:10.22 /usr/local/bin /usr/local/bin
COPY --from=node:10.22 /usr/local/lib /usr/local/lib

# パッケージ管理ツール(apt)-getの更新と各パッケージやphp拡張モジュールのインストール
RUN apt-get update \
    && apt-get install -y \
    git \
    zip \
    unzip \
    vim \
    libicu-dev \
    libzip-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd \
    && docker-php-ext-install zip \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-install pdo_mysql bcmath 

# コンテナでの作業ディレクトリを指定(docker-compose exec app bashを利用したときのカレントディレクトリになる)
WORKDIR /var/www/html
