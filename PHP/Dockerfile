FROM rodrigobp/debian-base

LABEL   version="0.1" \
        maintainer="Rodrigo Braga Prado <docker@quati.com.br>" \
        Description="latest stable php image with magento 2 confs"

#############
# Variables #
#############
ENV XDEBUG_HOST fpm_xdebug
ENV FPM_HOST fastcgi_backend
ENV FPM_PORT 9000
ENV UPSTREAM_HOST nginx
ENV UPSTREAM_PORT 80
ENV MAGENTO_ROOT /app
ENV MAGENTO_RUN_MODE production
ENV MFTF_UTILS 0
ENV DEBUG false

ENV PHP_MEMORY_LIMIT 2G
ENV PHP_MAX_EXECUTION_TIME 1800
ENV PHP_ZLIB_OUTPUT_COMPRESSION On
ENV PHP_UPLOAD_MAX_FILESIZE 1000M
ENV PHP_POST_MAX_SIZE 1100M
ENV PHP_MAX_INPUT_VARS 3000
ENV PHP_MAX_INPUT_TIME 1000

ENV PHP_OPCACHE_VALIDATE_TIMESTAMPS 0
ENV PHP_OPCACHE_MAX_ACCELERATED_FILES 60000
ENV PHP_OPCACHE_MAX_WASTED_PERCENTAGE 10
ENV PHP_OPCACHE_MEMORY_CONSUMPTION 512MB
ENV PHP_OPCACHE_CONSISTENCY_CHECKS 0
ENV PHP_OPCACHE_ENABLE_CLI 1

ENV PHP_REALPATH_CACHE_SIZE 10M
ENV PHP_REALPATH_CACHE_TTL 7200

EXPOSE 9000

########################
# Install dependencies #
########################
RUN apt-get update && \
    apt-get dist-upgrade -y &&  \
    apt-get install curl vim sudo wget gnupg2 ca-certificates apt-transport-https dialog tasksel bash -y && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys B188E2B695BD4743 && \
    wget -q https://packages.sury.org/php/apt.gpg -O- | apt-key add - && \
    echo "deb https://packages.sury.org/php/ buster main" > /etc/apt/sources.list.d/php.list && \
    apt-get update && \
    apt-get install curl vim sudo wget gnupg2 ca-certificates apt-transport-https dialog tasksel bash -y && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys B188E2B695BD4743 && \
    wget -q https://packages.sury.org/php/apt.gpg -O- | apt-key add - && \
    echo "deb https://packages.sury.org/php/ buster main" > /etc/apt/sources.list.d/php.list && \
    mkdir -p /run/php && \
    apt-get update && \
    apt-get install php-sodium php-apcu phpunit libperl-dev libxml-libxml-perl libxml2-dev libxml2-utils python-libxml2 libxslt1.1 libxslt1-dev python-libxslt1 libxml-filter-xslt-perl libxml-libxslt-perl libgd-dev libgd-perl python-gd libgoogle-perftools4 libgoogle-perftools-dev -y

##############
# Requisites #
##############
RUN mkdir -p /run/php && \
    apt-get update  && \
    apt-get install php-sodium php-apcu phpunit libperl-dev libxml-libxml-perl libxml2-dev libxml2-utils python-libxml2 libxslt1.1 libxslt1-dev python-libxslt1 libxml-filter-xslt-perl libxml-libxslt-perl libgd-dev libgd-perl python-gd libgoogle-perftools4 libgoogle-perftools-dev -y

####################
# PHP and Modules #
###################
RUN apt-get install php7.4-redis php7.4-memcache php7.4-amqp php7.4-fpm php7.4-common php7.4-bz2 php7.4-mysql php7.4-gd php7.4-xml php7.4-xmlrpc php7.4-curl php7.4-gd php7.4-cli php7.4-dev php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap php7.4-zip php7.4-intl php7.4-bcmath php7.4-dom php7.4-iconv php7.4-simplexml php7.4-xsl php7.4-imagick php7.4-ctype php7.4-xdebug -y

#########################
# Configure php modules #
#########################
# PHP.INI #
###########
RUN sed -i 's/memory_limit = .*/memory_limit = '${PHP_MEMORY_LIMIT}'/' /etc/php/7.4/fpm/php.ini && \
    sed -i 's/max_execution_time = .*/max_execution_time = '${PHP_MAX_EXECUTION_TIME}'/' /etc/php/7.4/fpm/php.ini && \
    sed -i 's/zlib.output_compression = .*/zlib.output_compression = '${PHP_ZLIB_OUTPUT_COMPRESSION}'/' /etc/php/7.4/fpm/php.ini && \
    sed -i 's/upload_max_filesize = .*/upload_max_filesize = '${PHP_UPLOAD_MAX_FILESIZE}'/' /etc/php/7.4/fpm/php.ini && \
    sed -i 's/post_max_size = .*/post_max_size = '${PHP_POST_MAX_SIZE}'/' /etc/php/7.4/fpm/php.ini && \
    sed -i 's/max_input_vars = .*/max_input_vars = '${PHP_MAX_INPUT_VARS}'/' /etc/php/7.4/fpm/php.ini && \
    sed -i 's/max_input_time = .*/max_input_time = '${PHP_MAX_INPUT_TIME}'/' /etc/php/7.4/fpm/php.ini && \
    sed -i 's/realpath_cache_size = .*/realpath_cache_size = '${PHP_REALPATH_CACHE_SIZE}'/' /etc/php/7.4/fpm/php.ini && \
    sed -i 's/realpath_cache_ttl = .*/realpath_cache_ttl = '${PHP_REALPATH_CACHE_TTL}'/' /etc/php/7.4/fpm/php.ini

##################
# PHP FPM Listen #
##################
RUN echo "listen = 0.0.0.0:9000" >> /etc/php/7.4/fpm/pool.d/www.conf

###################
# PHP Module APCU #
###################
RUN echo "[apcu]" >>/etc/php/7.4/fpm/conf.d/20-apcu.ini && \
    echo "apc.enabled = 1 " >>/etc/php/7.4/fpm/conf.d/20-apcu.ini

######################
# PHP Module OPCache #
######################
RUN echo "[opcache]" >> /etc/php/7.4/fpm/conf.d/10-opcache.ini && \
    echo "opcache.enable_cli=1" >> /etc/php/7.4/fpm/conf.d/10-opcache.ini && \
    echo "opcache.enable=1" >> /etc/php/7.4/fpm/conf.d/10-opcache.ini && \
    echo "opcache.revalidate_freq=0" >> /etc/php/7.4/fpm/conf.d/10-opcache.ini && \
    echo "opcache.consistency_checks=${PHP_OPCACHE_VALIDATE_TIMESTAMPS}" >> /etc/php/7.4/fpm/conf.d/10-opcache.ini && \
    echo "opcache.validate_timestamps=${PHP_OPCACHE_VALIDATE_TIMESTAMPS}" >> /etc/php/7.4/fpm/conf.d/10-opcache.ini && \
    echo "opcache.max_accelerated_files=${PHP_OPCACHE_MAX_ACCELERATED_FILES}" >> /etc/php/7.4/fpm/conf.d/10-opcache.ini && \
    echo "opcache.memory_consumption=${PHP_OPCACHE_MEMORY_CONSUMPTION}" >> /etc/php/7.4/fpm/conf.d/10-opcache.ini && \
    echo "opcache.max_wasted_percentage=${PHP_OPCACHE_MAX_WASTED_PERCENTAGE}" >> /etc/php/7.4/fpm/conf.d/10-opcache.ini && \
    echo "opcache.interned_strings_buffer=16" >> /etc/php/7.4/fpm/conf.d/10-opcache.ini && \
    echo "opcache.fast_shutdown=1" >> /etc/php/7.4/fpm/conf.d/10-opcache.ini

RUN apt-get purge apache2* -y && apt-get clean -y

RUN service php7.4-fpm restart

CMD ["php7.4-fpm"]

