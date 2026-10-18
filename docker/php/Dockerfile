FROM php:8.4-fpm

# Instalador de extensões PHP (evita lidar com dependências de sistema na mão)
COPY --from=ghcr.io/mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/
RUN install-php-extensions pdo_pgsql pgsql pcntl bcmath intl zip opcache

# Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

RUN apt-get update \
    && apt-get install -y --no-install-recommends git unzip \
    && rm -rf /var/lib/apt/lists/*

# Usuário com o mesmo UID do seu usuário no WSL (evita arquivos "de root" no projeto)
ARG HOST_UID=1000
ARG HOST_GID=1000
RUN groupadd -g ${HOST_GID} app && useradd -u ${HOST_UID} -g app -m app

WORKDIR /var/www/html
USER app