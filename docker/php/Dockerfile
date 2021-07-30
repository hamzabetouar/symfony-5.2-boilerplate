FROM php:8-fpm-buster

ARG TIMEZONE='America/New_York'

COPY docker/php/php.ini /usr/local/etc/php/conf.d/docker-php-config.ini

RUN apt-get update && apt-get install -y \
    gnupg \
    g++ \
    procps \
    openssl \
    git \
    unzip \
    zlib1g-dev \
    libzip-dev \
    libfreetype6-dev \
    libpng-dev \
    libjpeg-dev \
    libicu-dev  \
    libonig-dev \
    libxslt1-dev \
    acl \
    jpegoptim
    RUN echo 'alias sfc="php bin/console"' >> ~/.bashrc

RUN docker-php-ext-configure gd
RUN docker-php-ext-install \
        pdo \
        pdo_mysql \
        zip \
        xsl \
        gd \
        intl \
        opcache \
        exif \
        mbstring

#RUN apt-get update \
#    && apt-get install -y libmagickwand-dev --no-install-recommends \
#    && rm -rf /var/lib/apt/lists/* \
#    && pecl install imagick \
#	&& docker-php-ext-enable imagick

#RUN apt-get update
#RUN apt-get install -y --no-install-recommends libnss3-dev
#RUN rm -rf /var/lib/apt/lists/*
#RUN curl -LO https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
#RUN apt-get install -y ./google-chrome-stable_current_amd64.deb
#RUN rm google-chrome-stable_current_amd64.deb
#RUN curl -o /tmp/chromedriver.zip http://chromedriver.storage.googleapis.com/`curl -sS chromedriver.storage.googleapis.com/LATEST_RELEASE`/chromedriver_linux64.zip
#RUN unzip /tmp/chromedriver.zip chromedriver -d /usr/bin/
#RUN rm /tmp/chromedriver.zip

## Chromedriver ##
# We need wget to set up the PPA and xvfb to have a virtual screen and unzip to install the Chromedriver
RUN apt-get install -y wget xvfb unzip
# Set up the Chrome PPA
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
RUN echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list
# Update the package list and install chrome
RUN apt-get update -y
RUN apt-get install -y google-chrome-stable
RUN rm -rf /var/lib/apt/lists/*
# Download and install Chromedriver
RUN curl -o /tmp/chromedriver.zip http://chromedriver.storage.googleapis.com/`curl -sS chromedriver.storage.googleapis.com/LATEST_RELEASE`/chromedriver_linux64.zip
RUN unzip /tmp/chromedriver.zip chromedriver -d /usr/bin/
RUN rm /tmp/chromedriver.zip
####

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini
RUN "date"

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer self-update --2

# install node js
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get install -y nodejs
RUN npm install -g yarn

# Install Symfony CLI
RUN apt-get install -y bash \
    && wget https://get.symfony.com/cli/installer -O - | bash \
    && mv /root/.symfony/bin/symfony /usr/local/bin/symfony

WORKDIR /var/www/codebase
