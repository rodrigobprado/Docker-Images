FROM rodrigobp/debian-base:latest 

LABEL   version="0.1b" \
        maintainer="Rodrigo Braga Prado <docker@quati.com.br>" \
        Description="latest stable nginx image with magento 2 confs"

#############
# Variables #
#############
ENV PHP_HOST php
ENV UPLOAD_MAX_FILESIZE 600M
ENV XDEBUG_HOST fpm_xdebug
ENV FPM_HOST fastcgi_backend
ENV FPM_PORT 9000
ENV UPSTREAM_HOST nginx
ENV UPSTREAM_PORT 80
ENV MAGENTO_ROOT /app
ENV MAGENTO_RUN_MODE production
ENV MFTF_UTILS 0
ENV DEBUG false

VOLUME ${MAGENTO_ROOT}
WORKDIR ${MAGENTO_ROOT}

EXPOSE 80 8080

#################
# Install nginx #
#################
RUN apt-get update && \
    apt-get dist-upgrade -y && \
    apt-get install nginx nginx-extras dnsutils systemd composer -y

RUN mkdir -p ${MAGENTO_ROOT} && \
    mkdir -p /etc/nginx/magento/

RUN  rm /etc/nginx/sites-enabled/* 
RUN  ln -s /etc/nginx/sites-available/magento.conf /etc/nginx/sites-enabled/magento.conf

ENTRYPOINT /usr/sbin/nginx -c /etc/nginx/nginx.conf  -g 'daemon off;' 
