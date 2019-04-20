FROM php:7.1-apache	
# or php:7.0-apache, php:5.6-apache @see https://github.com/docker-library/php

# GD library install
RUN apt-get update -y && apt-get install -y sendmail libpng-dev

RUN apt-get update && \
    apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install gd

RUN docker-php-ext-install mbstring
RUN docker-php-ext-install zip

# install imagick // @see https://gist.github.com/tristanlins/4c1da2508f0326a042aa
#RUN apt-get update \
 #   && apt-get install -y \
  #      libmagickwand-dev \
 #   && rm -rf /var/lib/apt/lists/* \
 #   && pecl install imagick-beta \
 #   && echo "extension=imagick.so" > /usr/local/etc/php/conf.d/ext-imagick.ini \
 #   && apt-get remove -y \
 #       libmagickwand-dev \
 #   && apt-get install -y \
 #       libmagickwand-6.q16-2 \
 #   && apt-get autoremove -y

RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/ssl-cert-snakeoil.key -out /etc/ssl/certs/ssl-cert-snakeoil.pem -subj "/C=AT/ST=Vienna/L=Vienna/O=Security/OU=Development/CN=example.com"
RUN a2enmod ssl

RUN a2enmod rewrite

RUN docker-php-ext-install mysqli pdo pdo_mysql

# PHP config
RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini
RUN sed -i 's/memory_limit = 128M/memory_limit = 2G/' /usr/local/etc/php/php.ini

# Xdebug config
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
RUN echo 'xdebug.remote_enable = 1\n\
xdebug.remote_handler=dbgp\n\
xdebug.remote_mode=req\n\
xdebug.remote_host="host.docker.internal"\n\
xdebug.remote_autostart=0\n\
xdebug.profiler_enable_trigger=1\n'\
>> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# enable mail sending for php with ssmtp
#RUN apt-get update && \
#  apt-get install -y ssmtp && \
#  apt-get clean && \
#  rm -r /var/lib/apt/lists/* && \
#  echo "FromLineOverride=YES" >> /etc/ssmtp/ssmtp.conf && \
#  echo 'sendmail_path = "/usr/sbin/ssmtp -t"' > /usr/local/etc/php/conf.d/mail.ini

# enable mail sending for php with sendmail
#RUN apt-get update && apt-get upgrade -y sendmail && rm -rf /var/lib/apt/lists/* && \
#    echo 'sendmail_path = "/usr/sbin/sendmail -t -i"' > /usr/local/etc/php/conf.d/mail.ini

#RUN echo "$(hostname)" >> /etc/mail/relay-domains
#RUN m4 /etc/mail/sendmail.mc > /etc/mail/sendmail.cf
#RUN echo "127.0.0.1 noreply.domain.com $(hostname)" >> /etc/hosts
#RUN service sendmail restart
