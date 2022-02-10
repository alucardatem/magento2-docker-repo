FROM php:7.4-fpm
RUN apt-get update && apt-get install -y \
  libonig-dev \
  cron \
  git \
  libfreetype6-dev \
  libicu-dev \
  libjpeg62-turbo-dev \
  libmagickwand-dev \
  libmcrypt-dev \
  libpng-dev \
  libxslt1-dev \
  default-mysql-client \
  vim \
  zip \
  libzip-dev \
  wget \
  net-tools \
  netcat \
  mc \
  iputils-ping \
  htop

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - \
    && apt-get install -y --no-install-recommends nodejs \
    && npm install -g grunt-cli


RUN docker-php-ext-configure gd \
--with-jpeg=/usr/include/ \
--with-freetype=/usr/include/

RUN docker-php-ext-install \
  bcmath \
  gd \
  gettext \
  intl \
  mbstring \
  opcache \
  pdo_mysql \
  soap \
  xsl \
  zip \
  sockets \
  exif

RUN pecl channel-update pecl.php.net \
  && pecl install xdebug \
  && docker-php-ext-enable xdebug \
  && sed -i -e 's/^zend_extension/\;zend_extension/g' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN pecl install imagick-3.4.3 \
  && docker-php-ext-enable imagick

RUN curl -sS https://getcomposer.org/installer | \
  php -- --install-dir=/usr/local/bin --filename=composer

RUN curl -sS https://getcomposer.org/installer | \
  php -- --install-dir=/usr/local/bin --version=1.10.16 --filename=composer1


RUN wget https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64 \
    && chmod +x mhsendmail_linux_amd64 \
    &&  mv mhsendmail_linux_amd64 /usr/local/bin/mhsendmail

COPY conf/php.ini /usr/local/etc/php/

WORKDIR /app

EXPOSE 9000