FROM nginx:latest

RUN apt-get clean && apt-get update && \
    apt-get install -y curl fcgiwrap libcgi-session-perl libimage-magick-perl nano spawn-fcgi tofrodos wget zip

RUN sed -i 's/www-data/nginx/g' /etc/init.d/fcgiwrap
RUN chown nginx:nginx /etc/init.d/fcgiwrap
ADD ./default.conf /etc/nginx/conf.d/default.conf

WORKDIR /var/www

RUN wget -q -O /var/www/yybbs.zip https://www.kent-web.com/dload/yybbs.zip && \
    unzip -o -qq -d /var/www /var/www/yybbs.zip && mv yybbs/* ./ && \
    rm -rf ./yybbs && rm -rf ./yybbs.zip && \
    find . -regex '.*\.cgi' -exec fromdos {} \; && \
    find . -regex '.*\.cgi' -exec sed -i 's#/usr/local/bin/perl#/usr/bin/perl#g' {} \; && \
    sed -i 's#../index.html#./yybbs.cgi#g' ./init.cgi && \
    chown -R nginx:nginx . && \
    chmod 777 ./ && \
    chmod 755 ./*.cgi && \
    chmod 644 ./init.cgi && \
    chmod 666 ./data/*.cgi && \
    chmod 777 ./data/past

CMD /etc/init.d/fcgiwrap start \
    && nginx -g 'daemon off;'
