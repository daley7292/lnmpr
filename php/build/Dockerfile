FROM php:8.2-fpm-alpine
RUN apk add --no-cache --virtual .build-deps $PHPIZE_DEPS libzip-dev freetype-dev libjpeg-turbo-dev libpng-dev libwebp-dev oniguruma-dev libssh2-dev \
    && apk add --no-cache bash supervisor ttf-dejavu libzip freetype libjpeg-turbo libpng libwebp libssh2 oniguruma \
    && pecl install redis ssh2 igbinary \
    && docker-php-ext-enable redis ssh2 igbinary \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install pdo_mysql mbstring exif pcntl zip gd bcmath \
    && curl -sSL -o ioncube.tar.gz https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin-musl_x86-64.tar.gz \
    && tar -xzf ioncube.tar.gz \
    && mv ioncube/ioncube_loader_lin-musl_8.2.so "$(php-config --extension-dir)/" \
    && echo "zend_extension=ioncube_loader_lin-musl_8.2.so" > /usr/local/etc/php/conf.d/00-ioncube.ini \
    && rm -rf ioncube.tar.gz ioncube \
    && apk del .build-deps

COPY ./supervisord.conf /etc/supervisord.conf

WORKDIR /

CMD ["supervisord", "-n", "-c", "/etc/supervisord.conf"]
