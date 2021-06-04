FROM php:7.4-apache


ENV XDEBUG = $XDEBUG
ENV COMPOSER_VERSION = $COMPOSER_VERSION

#basic stuffs
RUN apt-get update \
&& apt-get install -y \ 
libfreetype6-dev \
libjpeg62-turbo-dev \
libpng-dev \
libzip-dev


#PHP Extensions

RUN if [ "XDEBUG" = 1 ] ; then pecl install xdebug && docker-php-ext-enable xdebug ; fi


RUN docker-php-ext-configure gd --with-freetype --with-jpeg 
RUN docker-php-ext-install opcache gd pdo_mysql zip 
RUN docker-php-ext-enable opcache gd zip

#Apache
RUN a2enmod rewrite \
&& a2enmod headers \
&& a2enmod vhost_alias \
&& echo "ServerName localhost" >> /etc/apache2/apache2.conf \
&& service apache2 restart

#composer
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php \
&& php composer-setup.php --install-dir=/usr/local/bin --filename=composer --"COMPOSER_VERSION" \
&& rm composer-setup.php


#others
ADD entrypoint.sh /entrypoint.sh
CMD ["/entrypoint.sh"]
