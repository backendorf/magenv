ARG PHP_VERSION
ARG XDEBUG_VERSION
FROM php:${PHP_VERSION}

# Install system dependencies and extension installer
RUN apt-get update && apt-get install -y \
    unzip wget git vim sudo curl gpgv nodejs npm \
    && curl -sSL https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions -o /usr/local/bin/install-php-extensions \
    && chmod +x /usr/local/bin/install-php-extensions \
    && npm install grunt-cli -g \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions (Opted for simplicity over performance)
RUN install-php-extensions \
    pdo_mysql \
    gd \
    bcmath \
    intl \
    zip \
    sockets \
    xsl \
    soap \
    mysqli \
    ftp \
    pcntl \
    xdebug-${XDEBUG_VERSION}

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Configure PHP settings
ARG PHP_MEMORY_LIMIT
RUN echo "memory_limit = ${PHP_MEMORY_LIMIT}" >> /usr/local/etc/php/php.ini

# Xdebug configuration (disabled by default, enable with: mv ...xdebug.ini.off ...xdebug.ini)
RUN echo "xdebug.client_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.mode=off" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.start_with_request=trigger" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.force_display_errors=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Add aliases and helper scripts
COPY x /usr/bin/x
RUN chmod +x /usr/bin/x

# Create app user and configure environment
RUN groupadd -g 1000 app && \
    useradd -g 1000 -u 1000 -d /var/www -s /bin/bash app && \
    mkdir -p /var/www/html /var/www/.history && \
    chown -R app:app /var/www && \
    echo "app ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/app && \
    chmod 0440 /etc/sudoers.d/app

# Bash enhancements for developer experience
RUN echo 'PS1="\[\e[32m\]\u@magento2:\[\e[34m\]\w\[\e[0m\]\$ "' >> /var/www/.bashrc && \
    echo 'alias ls="ls --color=auto"' >> /var/www/.bashrc && \
    echo 'alias ll="ls -la"' >> /var/www/.bashrc

USER app
WORKDIR /var/www/html
