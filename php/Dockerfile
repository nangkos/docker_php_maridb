FROM php:7-apache

# Localtime을 UST에서 KST(Korea Standard Time)로 변경
RUN ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime

# 미러사이트를 ftp.kaist.ac.kr로 변경
RUN sed -i "s/archive.ubuntu.com/ftp.kaist.ac.kr/g" /etc/apt/sources.list

RUN apt-get update && apt-get install -y \
# Tools
  apt-utils \
  vim \
  curl \
  wget \
  git \
  zip \
  mysql-client \
# Configure PHP
  libfreetype6-dev \
  libjpeg62-turbo-dev \
  libmcrypt-dev \
  libpng-dev \
  libicu-dev \
  libxml2-dev \
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-png-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install -j$(nproc) gd iconv intl xml soap opcache pdo pdo_mysql mysqli mbstring

RUN apt-get install -y libssl-dev openssl \
  && docker-php-ext-install phar

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf
RUN sed -i "/<Directory \/var\/www\/>/, /<Directory>/ s/AllowOverride None/AllowOverride all/" /etc/apache2/apache2.conf

RUN a2enmod rewrite \
  && a2enmod headers \
  && apache2ctl -k graceful \
  && apt-get clean \
  && apt-get -y purge \
    libxml2-dev libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev libpng-dev

RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer && chmod +x /usr/local/bin/composer

VOLUME ["/var/www/html", "/usr/local/etc/php/conf.d"]