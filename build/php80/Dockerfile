FROM php:8.0-fpm-buster

COPY resource /home/resource

ARG CHANGE_SOURCE=true
ARG TIME_ZONE=UTC

ARG MongoDB=mongodb-1.12.1.tgz
ARG OPEN_SWOOLE=openswoole-4.10.0.tgz
ARG SWOOLE=swoole-4.8.7.tgz
ARG REDIS=redis-5.3.7.tgz
ARG MCRYPT=mcrypt-1.0.4.tgz
ARG XDEBUG=xdebug-3.1.3.tgz
ARG AMQP=amqp-1.9.4.tgz

ENV TIME_ZONE=${TIME_ZONE} LC_ALL=C.UTF-8

RUN set -eux; \
  # ⬇ 修改时区
  ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime ; \ 
  echo $TIME_ZONE > /etc/timezone ; \
  \
  # ⬇ 安装 PHP Composer
  mv /home/resource/composer.phar /usr/local/bin/composer ; \
  chmod 755 /usr/local/bin/composer ; \
  \
  # ⬇ 替换源
  rm -rf /etc/apt/sources.list.d/buster.list ; \
  if [ ${CHANGE_SOURCE} = true ]; then \
    mv /etc/apt/sources.list /etc/apt/source.list.bak; \ 
    mv /home/resource/sources.list /etc/apt/sources.list; \
    composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ ; \
  fi; \
  \
  # ⬇ 更新、安装基础组件
  apt-get update; \
  apt-get upgrade -y; \
  apt-get install -y --allow-remove-essential --no-install-recommends \
  # tools
  wget \
  zip \
  unzip \
  git \
  cron \
  vim \
  iputils-ping \
  telnet \
  net-tools \
  \
  libmagickwand-dev \
  libz-dev \
  # intl
  libicu-dev \
  # bz2
  libbz2-dev \
  # zip
  libzip-dev \
  # ldap
  libldb-dev \
  libldap2-dev \
  # mcrypt
  libmcrypt-dev \
  # other
  libtinfo5 \
  libpq-dev \
  libjpeg-dev \
  libpng-dev \
  libfreetype6-dev \
  libssl-dev \
  libfreetype6-dev \
  libjpeg62-turbo-dev \
  zlib1g-dev \
  libpng-dev \
  procps \
  libgmp-dev\
  libxml2-dev \
  libxslt-dev \
  python-dev\
  libmhash-dev \
  librabbitmq-dev \
  geoip-bin \
  geoip-database \
  libgeoip-dev\
  python-dev\
  ntpdate ;

####################################################################################
# 安装 PHP 扩展
####################################################################################

RUN docker-php-ext-configure gd \
  --with-freetype \
  --with-jpeg; \
  docker-php-ext-install gd;\
  docker-php-ext-install intl ;\
  docker-php-ext-install pdo_mysql ;\
  docker-php-ext-install ldap ;\
  docker-php-ext-install mysqli ;\
  docker-php-ext-install bz2 ;\
  docker-php-ext-install zip ;\
  docker-php-ext-install sockets ;\
  docker-php-ext-install pgsql ;\
  docker-php-ext-install pdo_pgsql ;\
  docker-php-ext-install bcmath ;\
  docker-php-ext-install soap ;\
  docker-php-ext-install pcntl ;\
  docker-php-ext-install calendar ;\
  docker-php-ext-install com_dotnet ;\
  docker-php-ext-install ctype ;\
  docker-php-ext-install curl ;\
  docker-php-ext-install date ;\
  docker-php-ext-install dom ;\
  docker-php-ext-install exif ;\
  docker-php-ext-install fileinfo ;\
  docker-php-ext-install filter ;\
  docker-php-ext-install gmp ;\
  docker-php-ext-install ftp ;\
  docker-php-ext-install gettext ;\
  docker-php-ext-install hash ;\
  docker-php-ext-install imap ;\
  docker-php-ext-install iconv ;\
  docker-php-ext-install json ;\
  docker-php-ext-install libxml ;\
  docker-php-ext-install mbstring ;\
  docker-php-ext-install mysqlnd ;\
  docker-php-ext-install openssl ;\
  docker-php-ext-install pcre ;\
  docker-php-ext-install pdo ;\
  docker-php-ext-install pdo_sqlite ;\
  docker-php-ext-install Phar ;\
  docker-php-ext-install posix ;\
  docker-php-ext-install readline ;\
  docker-php-ext-install Reflection ;\
  docker-php-ext-install session ;\
  docker-php-ext-install shmop ;\
  docker-php-ext-install SimpleXML ;\
  docker-php-ext-install SPL ;\
  docker-php-ext-install sqlite3 ;\
  docker-php-ext-install standard ;\
  docker-php-ext-install sysvmsg ;\
  docker-php-ext-install sysvsem ;\
  docker-php-ext-install sysvshm ;\
  docker-php-ext-install tokenizer ;\
  docker-php-ext-install xml ;\
  docker-php-ext-install xmlreader ;\
  docker-php-ext-install xmlwriter ;\
  docker-php-ext-install sodium;\
  docker-php-ext-install opcache;\
  docker-php-ext-install xsl;\
  docker-php-ext-install zlib;\
  # \
  # ⬇ XDebug
  # pecl install /home/resource/$XDEBUG ; \
  \
  # ⬇ Imagick
  pecl install imagick && docker-php-ext-enable imagick; \
  \
  # ⬇ MongoDB
  pecl install /home/resource/$MongoDB ; \
  echo "extension=mongodb.so" > /usr/local/etc/php/conf.d/mongodb.ini ; \
  \
  # ⬇ Redis
  pecl install /home/resource/$REDIS ; \
  echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini ; \
  \
  # ⬇ Amqp
  pecl install amqp; \
  echo "extension=amqp.so" > /usr/local/etc/php/conf.d/amqp.ini ; \
  \
  # ⬇ crypto
  pecl install channel://pecl.php.net/crypto-0.3.2; \
  echo "extension=crypto.so" > /usr/local/etc/php/conf.d/crypto.ini ; \
  \
  # ⬇ mcrypt
  pecl install mcrypt; \
  echo "extension=mcrypt.so" > /usr/local/etc/php/conf.d/mcrypt.ini ; \
  \
  # ⬇ msgpack
  pecl install msgpack; \
  echo "extension=msgpack.so" > /usr/local/etc/php/conf.d/msgpack.ini ; \
  \
  # ⬇ igbinary
  pecl install igbinary; \
  echo "extension=igbinary.so" > /usr/local/etc/php/conf.d/igbinary.ini ; \
  \
  # ⬇ geoip
  COPY /home/resource/geoip.so  /usr/local/lib/php/extensions/no-debug-non-zts-20200930/ \
  echo "extension=geoip.so" > /usr/local/etc/php/conf.d/geoip.ini ; \
  \
  # \
  # # ⬇ Open Swoole
  # pecl install /home/resource/$OPEN_SWOOLE ; \
  # echo "extension=openswoole.so" > /usr/local/etc/php/conf.d/openswoole.ini ; \
  \
  # ⬇ Swoole
  pecl install /home/resource/$SWOOLE ; \
  echo "extension=swoole.so" > /usr/local/etc/php/conf.d/swoole.ini ; \
  \
  # ⬇ Mcrypt
  pecl install /home/resource/$MCRYPT ; \
  echo "extension=mcrypt.so" > /usr/local/etc/php/conf.d/mcrypt.ini ; \
  \
  # ⬇ 清理
  rm -rf /var/lib/apt/lists/* ; \
  apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false ; \
  rm -rf /home/resource ;
