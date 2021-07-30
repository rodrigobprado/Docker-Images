FROM rodrigobp/debian-base

LABEL   version="0.2" \
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

RUN sed -i '/^bind/d' /etc/redis/redis.conf && \
    echo "bind 0.0.0.0" >> /etc/redis/redis.conf && \
    sed -i 's/lazyfree-lazy-eviction no/lazyfree-lazy-eviction yes/g' /etc/redis/redis.conf && \
    sed -i 's/lazyfree-lazy-expire no/lazyfree-lazy-expire yes/g' /etc/redis/redis.conf && \
    sed -i 's/lazyfree-lazy-server-del no/lazyfree-lazy-server-del yes/g' /etc/redis/redis.conf && \
    sed -i 's/replica-lazy-flush no/replica-lazy-flush yes/g' /etc/redis/redis.conf && \
    sed -i 's/repl-diskless-sync no/repl-diskless-sync yes/g' /etc/redis/redis.conf && \
    service redis-server restart

CMD ["redis-server"]
