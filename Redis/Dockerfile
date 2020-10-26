FROM rodrigobp/debian-base

LABEL   version="0.1" \
        maintainer="Rodrigo Braga Prado <docker@quati.com.br>" \
        Description="latest stable redis image with magento 2 confs"


VOLUME /data

EXPOSE 6379


########################
# Install dependencies #
########################
RUN apt-get update && \
    apt-get dist-upgrade -y &&  \
    apt-get install redis-server  redis-tools -y

CMD ["redis-server"]
