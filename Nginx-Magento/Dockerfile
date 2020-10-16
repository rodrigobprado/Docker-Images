FROM rodrigobp/debian-base:latest 

LABEL   version="0.1" \
        maintainer="Rodrigo Braga Prado <docker@quati.com.br>" \
        Description="latest stable nginx image with magento 2 confs"

#############
# Variables #
#############
ENV PHP_HOST php-fpm
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

EXPOSE 8080

#################
# Install nginx #
#################
RUN apt-get update && \
    apt-get dist-upgrade -y && \
    apt-get install nginx nginx-extras -y

#############################
# Configure PHP for Magento #
#############################
RUN echo -e "upstream fastcgi_backend { \
        server  ${PHP_HOST}:9000; \
      } \
      " > /etc/nginx/conf.d/php.conf

COPY nginx.conf.sample /etc/nginx/sites-avaliable/nginx.conf.sample
COPY magento.conf /etc/nginx/sites-avaliable/magento.conf

RUN    ln -s /etc/nginx/sites-avaliable/magento.conf /etc/nginx/sites-enabled/magento.conf


CMD ["nginx", "-g", "daemon off;"]
