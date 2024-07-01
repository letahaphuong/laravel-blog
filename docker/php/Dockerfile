FROM php:8.2-fpm-alpine

# Cập nhật và cài đặt các gói cần thiết
RUN apk update && apk add --no-cache tzdata \
    autoconf g++ make openssl-dev \
    libpng-dev libzip-dev postgresql-dev \
    linux-headers supervisor

# Cài đặt múi giờ
ENV TZ="Asia/Ho_Chi_Minh"

# Cài đặt các PHP extensions
RUN docker-php-ext-install gd zip bcmath sockets pcntl pdo_pgsql exif

# Cài đặt Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Cài đặt và kích hoạt các PHP extensions qua PECL
RUN pecl install redis xdebug && \
    docker-php-ext-enable redis xdebug pcntl

# Sao chép file cấu hình
COPY 90-xdebug.ini "${PHP_INI_DIR}"/conf.d

COPY conf/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY conf/crontab /etc/crontabs/root
COPY conf/www.conf /usr/local/etc/php-fpm.d/www.conf

# Tạo thư mục cho log của Supervisor
RUN mkdir -p /var/log/supervisor

# Mở cổng 443
EXPOSE 443

# Chạy Supervisor
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
