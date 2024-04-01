FROM php:8.2-fpm-bullseye

RUN apt-get update

# Add required deb repos
RUN apt-get install -y ca-certificates curl gnupg \
    && mkdir -p /etc/apt/keyrings \
    && curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg \
    && echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_16.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list \
    && apt-get update

# Install dependencies
RUN apt-get install -y git zlib1g-dev libpng-dev libfreetype6-dev libjpeg62-turbo-dev libavif-dev libicu-dev libcurl4-gnutls-dev libonig-dev libzip-dev unzip nodejs

RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y libzip-dev libjpeg-dev libpng-dev libjpeg62-turbo-dev libicu-dev nodejs \
    && pecl install -o -f redis xdebug \
    && rm -rf /tmp/pear \
    && docker-php-ext-configure intl \
    && docker-php-ext-configure gd --enable-gd --with-jpeg \
    && docker-php-ext-install mysqli pdo pdo_mysql zip exif gd intl \
    && docker-php-ext-enable redis xdebug

# Setup Chrome
RUN npm install -g npm@^9 \
    && apt-get install -y gconf-service libasound2 libatk1.0-0 libc6 libcairo2 libcups2 libdbus-1-3 libexpat1 libfontconfig1 libgbm1 libgcc1 libgconf-2-4 libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 libnspr4 libpango-1.0-0 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 ca-certificates fonts-liberation libappindicator1 libnss3 lsb-release xdg-utils wget libgbm-dev libxshmfence-dev chromium \
    && npm install --location=global --unsafe-perm puppeteer \
    && apt-get clean

COPY --from=composer /usr/bin/composer /usr/bin/composer