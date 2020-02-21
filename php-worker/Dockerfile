ARG PHP_VERSION
FROM php:7.4-alpine

ARG PHP_VERSION

RUN apk --update add wget \
  curl \
  git \
  build-base \
  libmemcached-dev \
  libmcrypt-dev \
  libxml2-dev \
  pcre-dev \
  zlib-dev \
  autoconf \
  cyrus-sasl-dev \
  libgsasl-dev \
  oniguruma-dev \
  supervisor

RUN docker-php-ext-install mysqli mbstring pdo pdo_mysql tokenizer xml pcntl
RUN pecl channel-update pecl.php.net && pecl install memcached mcrypt-1.0.1 mongodb && docker-php-ext-enable memcached mongodb

# Add a non-root user:
ARG PUID=1000
ARG PGID=1000

RUN addgroup -g ${PGID} caddock && \
    adduser -D -G caddock -u ${PUID} caddock

RUN docker-php-ext-install bcmath

RUN apk --update add postgresql-dev && \
    docker-php-ext-install pdo_pgsql

RUN set -eux && \
    apk --update add libzip-dev && \
    docker-php-ext-configure zip && \
    # Install the zip extension
    docker-php-ext-install zip

RUN printf "\n" | pecl install -o -f redis && \
    rm -rf /tmp/pear && \
    docker-php-ext-enable redis

#
#--------------------------------------------------------------------------
# Optional Supervisord Configuration
#--------------------------------------------------------------------------
#
# Modify the ./supervisor.conf file to match your App's requirements.
# Make sure you rebuild your container with every change.
#

COPY supervisord.conf /etc/supervisord.conf

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]

RUN php -v | head -n 1 | grep -q "PHP 7.4."

# Clean up
RUN rm /var/cache/apk/* \
    && mkdir -p /var/www

WORKDIR /etc/supervisor/conf.d/
