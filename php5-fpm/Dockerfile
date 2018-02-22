FROM helder/php-5.3
ARG TIMEZONE

RUN apt-get update && apt-get install -y \
    openssl \
    git \
    unzip \
    php5-memcache \
    memcached \
    php5-memcached \
    libxml2-dev \
    libmcrypt-dev \
    libmagickwand-dev \
    libmagickcore-dev \
    php5-imagick \
    php5-mcrypt \
    imagemagick libz-dev libmemcached-dev libmemcached11 libmemcachedutil2 build-essential \
  && pecl install memcache \
  && pecl install imagick-3.3.0 \
  && docker-php-ext-configure soap --enable-soap \
  && docker-php-ext-install pdo_mysql soap mysqli zip bcmath bz2 calendar mbstring mcrypt \
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
RUN echo extension=imagick.so >> /usr/local/etc/php/conf.d/imagick.ini
RUN echo extension=memcache.so >> /usr/local/etc/php/conf.d/memcached.ini

RUN mkdir /usr/local/rctdev
RUN mkdir /usr/local/pbodev
RUN mkdir /usr/local/gsdev
RUN mkdir /usr/local/ehs

RUN chmod 777 /usr/local/rctdev
RUN chmod 777 /usr/local/pbodev
RUN chmod 777 /usr/local/gsdev
RUN chmod 777 /usr/local/ehs

WORKDIR /var/www
