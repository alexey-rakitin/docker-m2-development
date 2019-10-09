FROM php:7.2-fpm

ARG MAGENTO_PATH
ARG USER_ID=1000
ARG GROUP_ID=1000

RUN apt-get update && apt-get install -y \
    apt-utils \
    sudo \
    wget \
    unzip \
    cron \
    curl \
    libmcrypt-dev \
    libicu-dev \
    libxml2-dev libxslt1-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    git \
    nano \
    mc \
    openssh-server \
    mysql-client \
    gnupg2 \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure hash --with-mhash \
    && docker-php-ext-install -j$(nproc) intl xsl gd zip pdo_mysql opcache soap bcmath json iconv \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && pecl install xdebug && docker-php-ext-enable xdebug \
    && mkdir /var/run/sshd \
    && curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash - && apt-get install -y nodejs \
    && echo "StrictHostKeyChecking no" >> /etc/ssh/ssh_config \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN curl -Lsf 'https://storage.googleapis.com/golang/go1.13.1.linux-amd64.tar.gz' | tar -C '/usr/local' -xvzf -
ENV PATH /usr/local/go/bin:$PATH
RUN go get github.com/mailhog/mhsendmail \
    && cp /root/go/bin/mhsendmail /usr/bin/mhsendmail
COPY ./auth.json /var/www/.composer/auth.json
COPY ./zphp.ini /usr/local/etc/php/conf.d/zphp.ini

RUN mkdir -p /var/www/.composer/cache/repo \
    && userdel -f www-data \
    && if getent group www-data ; then groupdel www-data; fi \
    && groupadd -g ${GROUP_ID} www-data \
    && useradd -l -u ${USER_ID} -g www-data www-data \
    && install -d -m 0755 -o www-data -g www-data /var/www \
    && chown --changes --silent --no-dereference --recursive ${USER_ID}:${GROUP_ID} /var/www/ \
    && usermod -d /var/www/ www-data
USER www-data
WORKDIR ${MAGENTO_PATH}

CMD ["php-fpm"]
