FROM rodrigobp/debian-base

LABEL   version="0.1" \
        maintainer="Rodrigo Braga Prado <docker@quati.com.br>" \
        Description="latest stable elasticsearch with magento 2 confs"

RUN apt-get update && \
    apt-get dist-upgrade -y && \
    apt-get install curl vim sudo wget gnupg2 ca-certificates apt-transport-https dialog tasksel bash procps -y

RUN wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add - && \
    apt-get install -y apt-transport-https 

RUN echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | tee /etc/apt/sources.list.d/elastic-7.x.list && \
    apt-get update && apt-get install -y elasticsearch && \
    update-rc.d elasticsearch defaults 95 10

RUN echo "network.bind_host: 0.0.0.0 
          network.host: 0.0.0.0
          http.host: 0.0.0.0
          network.publish_host: 0.0.0.0
          path.data: /var/lib/elasticsearch
          path.logs: /var/log/elasticsearch
          node.master: true
          node.data: true
          transport.host: 0.0.0.0
          transport.tcp.port: 9300 " > /etc/elasticsearch/elasticsearch.yml
          
EXPOSE 9200 9300

RUN service elasticsearch restart

CMD ["elasticsearch"]
