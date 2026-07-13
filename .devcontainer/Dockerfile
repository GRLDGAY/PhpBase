FROM php:8.4-apache

RUN apt-get update \
    && apt-get install -y git zip unzip \
    && docker-php-ext-install pdo_mysql \
    && a2enmod rewrite \
    && rm -rf /var/lib/apt/lists/*

RUN sed -i 's#/var/www/html#/workspace/projetPhp#g' \
    /etc/apache2/sites-available/000-default.conf

RUN printf '<Directory /workspace/projetPhp>\n\
    AllowOverride All\n\
    Require all granted\n\
</Directory>\n' \
    > /etc/apache2/conf-available/projetphp.conf \
    && a2enconf projetphp

WORKDIR /workspace/projetPhp
