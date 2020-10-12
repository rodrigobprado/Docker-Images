FROM nginx:latest 

LABEL   version="0.1" \
        maintainer="Rodrigo Braga Prado <docker@quati.com.br>" \
        Description="latest stable official nginx image with magento 2 confs"

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

EXPOSE 80

########################
# Install nginx #
########################
RUN apt-get update && \
    apt-get install nginx nginx-extras -y

#############################
# Configure PHP for Magento #
#############################
RUN echo -e "upstream fastcgi_backend { \
        server  ${PHP_HOST}:9000; \
      }" > /etc/nginx/conf.d/php.conf

#COPY etc/nginx.conf /etc/nginx/
#COPY etc/vhost.conf /etc/nginx/conf.d/default.conf
#COPY etc/xdebug-upstream.conf /etc/nginx/conf.d/xdebug/upstream.conf



#COPY docker-entrypoint.sh /docker-entrypoint.sh
#RUN ["chmod", "+x", "/docker-entrypoint.sh"]
#ENTRYPOINT ["/docker-entrypoint.sh"]


CMD ["nginx", "-g", "daemon off;"]
