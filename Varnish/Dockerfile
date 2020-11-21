 
FROM rodrigobp/debian-base

LABEL   version="0.1" \
        maintainer="Rodrigo Braga Prado <docker@quati.com.br>" \
        Description="latest stable varnish image with magento 2 confs"

RUN apt-get update && \
    apt-get dist-upgrade -y &&  \
    apt-get install varnish -y

# COPY default.vcl /etc/varnish/

CMD /usr/sbin/varnishd -F -f /etc/varnish/default.vcl
