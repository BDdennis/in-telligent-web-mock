FROM php:5.6.40-apache-stretch

# Permissions
RUN groupadd www -g12345 && usermod -aG 12345 root && usermod -aG 12345 www-data

# Backup and clean source.list file
RUN cp /etc/apt/sources.list /etc/apt/sources.list.old && \
    cat /dev/null > /etc/apt/sources.list

# Fix the source.list for stretch
RUN printf "deb http://archive.debian.org/debian/ stretch main\n" > /etc/apt/sources.list && \
    printf "deb-src http://archive.debian.org/debian/ stretch main\n" >>  /etc/apt/sources.list && \
    printf "deb http://archive.debian.org/debian-security stretch/updates main\n" >>  /etc/apt/sources.list && \
    printf "deb-src http://archive.debian.org/debian-security stretch/updates main" >>  /etc/apt/sources.list

RUN apt-get -y update && apt-get upgrade -y

# Install tools && libraries
RUN apt-get -y install --fix-missing vim wget dialog \
    build-essential git curl libcurl3 libcurl3-dev zip \
    libmcrypt-dev libsqlite3-dev libsqlite3-0 mysql-client \
    zlib1g-dev libicu-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    && rm -rf /var/lib/apt/lists/*

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# PHP5 Extensions
RUN docker-php-ext-install curl \
    && docker-php-ext-install tokenizer \
    && docker-php-ext-install json \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install mcrypt \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install zip \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && pecl install xdebug-2.5.5 && docker-php-ext-enable xdebug \
    && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/php.ini

# Insure an SSL directory exists
RUN mkdir -p /etc/apache2/ssl

# Enable SSL support
RUN a2enmod ssl && a2enmod rewrite

RUN a2enmod deflate && a2enmod expires

# Enable apache modules
RUN a2enmod rewrite headers

RUN a2dissite 000-default
COPY ./apache2.conf  /etc/apache2/apache2.conf
RUN echo 'ServerName localhost' >> /etc/apache2/apache2.conf
COPY ./config/vhosts/vhost.conf /etc/apache2/sites-available/
RUN a2ensite vhost
ENV APACHE_DOCUMENT_ROOT /var/www/html/
ENV APACHE_RUN_USER=www-data
ENV APACHE_RUN_GROUP=www
RUN chown www-data:www -R /var/www/html/

# PHP
COPY ./config/php/php.ini /usr/local/etc/php/php.ini

# Log
RUN chmod 777 -R /var/log/apache2 && chown www-data:www-data -R /var/log/apache2 && chmod 777 -Rf /var/www/html/ && chgrp www -R /var/www/html/

USER root:www

EXPOSE 80
# EXPOSE 443

ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]