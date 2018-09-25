FROM php:5.6-fpm
ARG TIMEZONE

RUN apt-get update && apt-get install -y \
    openssl \
    git \
    unzip \
    memcached \
    libxml2-dev \
    libmcrypt-dev \
    libmagickwand-dev \
    imagemagick

RUN apt-get -y install libz-dev libmemcached-dev libmemcached11 libmemcachedutil2 build-essential \
  && pecl install memcache \
  && pecl install imagick \
  && echo extension=memcache.so >> /usr/local/etc/php/conf.d/memcached.ini \
  && docker-php-ext-enable imagick \
  && docker-php-ext-configure soap --enable-soap \
  && docker-php-ext-install soap \
  && docker-php-ext-install mcrypt \
  && docker-php-ext-install mysql \
  && docker-php-ext-install mysqli \
  && docker-php-ext-install zip \
  && apt-get remove -y build-essential libmemcached-dev libz-dev \
  && apt-get autoremove -y \
  && apt-get clean \
  && rm -rf /tmp/pear

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini
RUN "date"

# Increase upload limits in ini file
RUN echo "upload_max_filesize = 250M" >> /usr/local/etc/php/conf.d/upload-max-size.ini
RUN echo "memory_limit = 300M" >> /usr/local/etc/php/conf.d/upload-max-size.ini
RUN echo "post_max_size = 255M" >> /usr/local/etc/php/conf.d/upload-max-size.ini

#enable php extensions
RUN echo "extension=imagick.so" >> /usr/local/etc/php/conf.d/imagick.ini

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install pdo pdo_mysql

# install xdebug
#RUN pecl install xdebug
#RUN docker-php-ext-enable xdebug
#RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN mkdir /usr/local/rctdev
RUN mkdir /usr/local/pbodev
RUN mkdir /usr/local/gsdev


RUN chmod 777 /usr/local/rctdev
RUN chmod 777 /usr/local/pbodev
RUN chmod 777 /usr/local/gsdev

WORKDIR /var/www
