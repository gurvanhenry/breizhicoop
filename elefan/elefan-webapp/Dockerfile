FROM php:7.2-apache

RUN apt-get update && apt-get install -y \
    unzip \
    libpng-dev

# Installation de Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Installation des extensions nécessaires
RUN docker-php-ext-install pdo_mysql pcntl gd

# Activtaion du Rewrite Mode pour Apache
RUN a2enmod rewrite

# Copie la web-app
COPY --chown=www-data:www-data ./gestion-compte/ /var/www/html/

# Copie le fichier parameters.yml
COPY --chown=www-data:www-data parameters.yml /var/www/html/app/config/parameters.yml

# Edite le fichier apache pour servir le dossier /var/www/html/web
RUN sed -i 's#/var/www/html#/var/www/html/web#g' /etc/apache2/sites-available/000-default.conf