FROM composer

LABEL maintainer="jason@ciandt.com"

ENV DRUSH_VERSION 9.4.0
ENV DRUSH_LAUNCHER_VERSION 0.6.0
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
    # Install Drush using Composer.
    && composer global require drush/drush:"$DRUSH_VERSION" --prefer-dist \
    && curl -fsSL -o /usr/local/bin/drush "https://github.com/drush-ops/drush-launcher/releases/download/$DRUSH_LAUNCHER_VERSION/drush.phar" \
    && chmod +x /usr/local/bin/drush

# Update the entry point for drush
ENTRYPOINT ["drush"]