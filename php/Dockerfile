FROM php:7-apache

RUN DEBIAN_FRONTEND=noninteractive apt-get update -q \
  && DEBIAN_FRONTEND=noninteractive apt-get dist-upgrade -y \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y \
  libfreetype6-dev \
  libmcrypt-dev \
  libpng12-dev \
  libcurl4-nss-dev \
  curl \
  && cd /tmp \
  && curl -L 'https://github.com/EasyFreeHost/pecl-memcache/archive/81b1267413662dfc276f4530122cd1e7aba5c1fc.tar.gz' | tar xz \
  && cd pecl-memcache-81b1267413662dfc276f4530122cd1e7aba5c1fc \
  && phpize \
  && ./configure \
  && make \
  && make install \
  && cd .. \
  && rm -Rf pecl-memcache-81b1267413662dfc276f4530122cd1e7aba5c1fc

# Install some neede packages
RUN apt-get update \
  && apt-get install -y --no-install-recommends libpq-dev libmcrypt-dev git unzip

# Install pdo_mysql
RUN apt-get update \
  && echo 'deb http://packages.dotdeb.org jessie all' >> /etc/apt/sources.list \
  && echo 'deb-src http://packages.dotdeb.org jessie all' >> /etc/apt/sources.list \
  && apt-get install -y wget \
  && wget https://www.dotdeb.org/dotdeb.gpg \
  && apt-key add dotdeb.gpg \
  && apt-get update \
  && apt-get install -y php7.0-mysql \
  && docker-php-ext-install pdo_mysql

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set timezone
RUN rm /etc/localtime
RUN ln -s /usr/share/zoneinfo/Europe/Paris /etc/localtime
RUN "date"

# create alias for symfony commands
RUN echo 'alias sf="php app/console"' >> ~/.bashrc
RUN echo 'alias sf3="php bin/console"' >> ~/.bashrc
