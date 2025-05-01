ARG UID
ARG GID

FROM php:8.4-fpm-alpine
RUN addgroup -g 1000 app && adduser -u 1000 -G app -s /bin/sh -D app

# RUN echo "UID is: ${UID}" && \
# echo "GID is: ${GID}" && \
# addgroup -g ${GID} app && \
# adduser -u ${UID} -G app -s /bin/sh -D app


RUN apk add --no-cache --update \
    libzip-dev \
    zip \
    unzip \
    git \
    icu-dev \
    oniguruma-dev

RUN docker-php-ext-install -j$(nproc) \
    pdo_mysql \
    intl \
    opcache \
    zip \
    bcmath \
    mbstring


# Create the php.ini file for FPM
RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini

# Ensure pdo_mysql is only loaded once in php.ini
RUN sed -i '/^extension=pdo_mysql\.so/d' /usr/local/etc/php/php.ini
RUN echo "extension=pdo_mysql.so" >> /usr/local/etc/php/php.ini

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

USER app
WORKDIR /var/www/html