# docker run -i -t -p 82:80 -p 1122:22 -v D:/docker/ww-lnmp/conf.d/:/etc/nginx/conf.d/ -v C:/MyWamp/www/:/var/www/html/ --name p711 -d php-fpm-nginx

FROM waan/php-fpm:7.2-alpine3.8

RUN apk add --no-cache nginx && mkdir /run/nginx \
    && mv /etc/inittab /etc/inittab.bak 

COPY inittab /etc/inittab
COPY rc.local /etc/rc.local

EXPOSE 80
EXPOSE 443
EXPOSE 9000

CMD ["/sbin/init"]
