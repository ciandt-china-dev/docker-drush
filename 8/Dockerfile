FROM php:7.1-alpine

LABEL maintainer="jason@ciandt.com"

ENV DRUSH_VERSION 8.1.16
ENV MEMCACHED_DEPS zlib-dev libmemcached-dev cyrus-sasl-dev

RUN set -xe \
    && apk --update add \
            build-base \
            libpq \
            mysql-client \
            postgresql-client \
            postgresql-dev \
            php7-pear \
            patch \
            tar && \
            docker-php-ext-install mysqli pgsql pdo_mysql pdo_pgsql && \
            apk del build-base postgresql-dev && \
            rm -rf /var/cache/apk/* \
    # Install memcache extension
    && apk add --no-cache --update libmemcached-libs zlib \
    && apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
    && apk add --no-cache --update --virtual .memcached-deps $MEMCACHED_DEPS \
    && pecl install memcached \
    && echo "extension=memcached.so" > /usr/local/etc/php/conf.d/20_memcached.ini \
    && rm -rf /usr/share/php7 \
    && rm -rf /tmp/* \
    && apk del .memcached-deps .phpize-deps \
    # Install Drush.
    && curl -fsSL -o /usr/local/bin/drush "https://github.com/drush-ops/drush/releases/download/$DRUSH_VERSION/drush.phar" \
    && chmod +x /usr/local/bin/drush \ 
    && drush --version

# Update the entry point for drush
ENTRYPOINT ["drush"]