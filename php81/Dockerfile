FROM php:8.1-apache

RUN apt-get update && apt-get -y install libicu-dev libaio-dev libxml2-dev libjpeg-dev libpng-dev python libfreetype6-dev libldap2-dev libzip-dev imagemagick vim less wget telnet cron sudo git npm unzip

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions

# Install Oracle extensions
RUN install-php-extensions pdo_oci ldap gd zip intl soap dom pdo_mysql exif opcache

RUN apt-get purge -y --auto-remove \
  && apt-get clean -y \
  && npm install -g yarn \
  && rm -rf /var/lib/apt/lists/* \
  && mkdir -p /var/www/html/public \
  && a2enmod headers rewrite \
  && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY _conf/charset.conf /etc/apache2/conf-available/charset.conf
COPY _php/timezone.ini /usr/local/etc/php/conf.d/timezone.ini
COPY _php/vars.ini /usr/local/etc/php/conf.d/vars.ini

RUN cp -f "/usr/local/etc/php/php.ini-production" /usr/local/etc/php/php.ini

WORKDIR /app

RUN touch /app/.env
RUN touch /etc/apache2/sites-available/000-default.conf
RUN mkdir /app/oracle_credentials/ && touch /app/oracle_credentials/tnsnames.ora

ENV TNS_ADMIN="/app/oracle_credentials/"