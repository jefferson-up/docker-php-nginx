# Define a versão do PHP baseada no argumento passado
ARG PHP_VERSION
FROM php:${PHP_VERSION}

## Diretório da aplicação
ARG APP_DIR=/var/www/app

## Versão da Lib do Redis para PHP
ARG REDIS_LIB_VERSION=5.3.7

### apt-utils é uma extensão de recursos do gerenciador de pacotes APT
RUN apt-get update -y && apt-get install -y --no-install-recommends \
    apt-utils \
    supervisor \
    cron \
    zlib1g-dev \
    libzip-dev \
    unzip \
    libpng-dev \
    libpq-dev \
    libxml2-dev \
    curl \
    gnupg \
    nano \
    libonig-dev \
    libssl-dev

# Instalação das extensões PHP recomendadas pela documentação da aplicação e necessárias para Laravel
RUN docker-php-ext-install pdo pdo_mysql mysqli session xml bcmath ctype mbstring fileinfo pcntl gd exif

# Habilita instalação do Redis
RUN pecl install redis-${REDIS_LIB_VERSION} && docker-php-ext-enable redis

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Configurações do Supervisor
COPY ./docker/supervisord/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY ./docker/supervisord/conf /etc/supervisord.d/

# Configurações adicionais do PHP
COPY ./docker/php/extra-php.ini "$PHP_INI_DIR/99_extra.ini"
COPY ./docker/php/extra-php-fpm.conf /etc/php8/php-fpm.d/www.conf

# Configura o diretório de trabalho e ajusta as permissões conforme a documentação
WORKDIR $APP_DIR
RUN chown -R www-data:www-data $APP_DIR
# Ajuste das permissões de diretório conforme recomendado na documentação
RUN chmod -R 777 $APP_DIR

# Instalação do Nginx e configuração
RUN apt-get install nginx -y
RUN rm -rf /etc/nginx/sites-enabled/* && rm -rf /etc/nginx/sites-available/*
COPY ./docker/nginx/sites.conf /etc/nginx/sites-enabled/default.conf
COPY ./docker/nginx/error.html /var/www/html/error.html

# Limpeza dos pacotes e cache do APT
RUN apt-get clean && rm -rf /var/lib/apt/lists/*
RUN apt update -y && apt install nano git -y

RUN nginx -t
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
