FROM ubuntu:18.04

MAINTAINER Jasper Carpizo <jcarpizo.cloud@gmail.com>

RUN apt-get update
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:ondrej/php
RUN add-apt-repository ppa:git-core/ppa
RUN apt-cache search php7.3

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update
RUN apt-get install -yq nginx supervisor \
    php7.3-fpm php7.3-bcmath php7.3-bz2 php7.3-cgi \
    php7.3-cli php7.3-common php7.3-curl php7.3-dba \
    php7.3-enchant php7.3-gd php7.3-gmp php7.3-imap \
    php7.3-interbase php7.3-intl php7.3-json php7.3-ldap \
    php7.3-mbstring php7.3-mysql php7.3-odbc php7.3-opcache \
    php7.3-pgsql php7.3-phpdbg php7.3-pspell php7.3-readline \
    php7.3-recode php7.3-snmp php7.3-soap php7.3-sqlite3 \
    php7.3-sybase php7.3-tidy php7.3-xml php7.3-xmlrpc \
    php7.3-xsl php7.3-zip \
    php-imagick php-memcache php-yaml php-apcu \
    php-phalcon php-xdebug git wget

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    php -r "if (hash_file('sha384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    php composer-setup.php \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer && \
    composer git

RUN wget -O phpunit https://phar.phpunit.de/phpunit-8.phar && \
    chmod +x phpunit && \
    mv phpunit /usr/local/bin/phpunit && \
    phpunit

RUN  rm -rf /var/lib/apt/lists/*

ENV NGINX_VHOST /etc/nginx/sites-available/default
ENV PHP_CONFIG /etc/php/7.3/fpm/php.ini
ENV NGINX_CONFIG /etc/nginx/nginx.conf
ENV SUPERVISOR_CONFIG /etc/supervisor/supervisord.conf
 
# Enable php-fpm on nginx virtualhost configuration
COPY default ${NGINX_VHOST}
RUN sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' ${PHP_CONFIG} && \
    echo "\ndaemon off;" >> ${NGINX_CONFIG}
 
#Copy supervisor configuration
COPY supervisord.conf ${SUPERVISOR_CONFIG}
 
RUN mkdir -p /run/php && \
    chown -R www-data:www-data /var/www/html && \
    chown -R www-data:www-data /run/php
 
# Volume configuration
VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"]
 
# Configure Services and Port
COPY start.sh /start.sh
CMD ["./start.sh"]
 
EXPOSE 80 443
