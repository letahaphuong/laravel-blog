FROM php:8.2-fpm-alpine

RUN apk update && apk add --no-cache tzdata

ENV TZ="Asia/Ho_Chi_Minh"

RUN apk add --update --no-cache supervisor

RUN apk add --update --no-cache autoconf g++ make openssl-dev
RUN apk add libpng-dev
RUN apk add libzip-dev
RUN apk add postgresql-dev
RUN apk add --update linux-headers

RUN docker-php-ext-install gd
RUN docker-php-ext-install zip
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install sockets
RUN docker-php-ext-install pcntl
RUN docker-php-ext-install pdo_pgsql
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN pecl install redis
RUN docker-php-ext-enable redis
RUN docker-php-ext-enable pcntl

RUN pecl install xdebug
COPY 90-xdebug.ini "${PHP_INI_DIR}"/conf.d

COPY conf/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY conf/crontab /etc/crontabs/root
COPY conf/www.conf /usr/local/etc/php-fpm.d/www.conf

RUN mkdir -p /var/log/supervisor

EXPOSE 443

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]