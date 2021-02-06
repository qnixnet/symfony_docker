ARG PHP_VERSION

# Install base image
FROM php:${PHP_VERSION}-fpm

# Install dependencies
RUN apt-get update && apt-get install -y \
    acl \
    git \
    procps \
    nano \
    unzip \
    zlib1g-dev \
    libicu-dev \
    libnss3-tools \
    libpng-dev \
    libpq-dev \
    libxslt-dev \
    libzip-dev

# Clear cache
RUN apt-get autoremove -y && apt-get autoclean -y && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-install \
    gd \
    intl \
    opcache \
    pdo_pgsql \
    xsl \
    zip

# Install & configure amqp extensions
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions && sync \
    && install-php-extensions \
        amqp \
        apcu \
        redis \
    && rm /usr/local/bin/install-php-extensions

# Configure PHP
COPY local_php.ini /usr/local/etc/php/conf.d/ext-symfony.ini

ARG TIMEZONE

RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
    && printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/ext-tzone.ini \
    && "date"

# Configure Git
ARG EMAIL_ADDRESS
ARG USER_NAME

RUN git config --global user.email "${EMAIL_ADDRESS}" \
    && git config --global user.name "${USER_NAME}"

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

# Install Symfomy CLI
RUN curl -sS https://get.symfony.com/cli/installer | bash \
    && mv /root/.symfony/bin/symfony /usr/local/bin/symfony

# Set working directory
WORKDIR /var/www/symfony

# Expose ports
EXPOSE 9000

# Run php-fpm
CMD ["php-fpm"]