FROM nginx
LABEL MAINTAINER="ilogachev@infomarketstudio.ru "
RUN apt-get update && apt-get install -y \
    nano \
    iputils-ping

COPY nginx.conf /etc/nginx/nginx.conf

WORKDIR /var/www/blog