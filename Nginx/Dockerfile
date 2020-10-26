FROM rodrigobp/debian-base:latest

################
# Base install #
################
RUN apt-get update && \
    apt-get dist-upgrade -y && \
    apt-get install -y \
    nginx && \
    nginx-extras && \
    apt-get clean

# Expose ports.
EXPOSE 80
EXPOSE 443    

CMD ["nginx", "-g", "daemon off;"]

