FROM appsvcorg/drupal-nginx-fpm:0.6
LABEL maintainer ="Azure App Service Container Images <appsvc-images@microsoft.com>"
# ========
# ENV vars
# ========
#
ENV DOCKER_BUILD_HOME "/dockerbuild"
# mariadb
ENV MARIADB_DATA_DIR "/home/data/mysql"
ENV MARIADB_LOG_DIR "/home/LogFiles/mysql"
# phpmyadmin
ENV PHPMYADMIN_SOURCE "/usr/src/phpmyadmin"
ENV PHPMYADMIN_HOME "/home/phpmyadmin"
#nginx
ENV NGINX_LOG_DIR "/home/LogFiles/nginx"
#varnish
ENV VARNISH_LOG_DIR "/home/LogFiles/varnish"
#php
ENV PHP_CONF_FILE "/usr/local/etc/php/php.ini"

RUN set -ex \
  && apk update \ 
  && apk add bash openjdk8 \
  && apk upgrade \
  && rm -rf /var/cache/apk/* \
  && rm -rf /etc/nginx/conf.d/default.conf 

# install drupal and mariadb

ENV DATABASE_TYPE local

# Apache Solr
RUN apk add apache-ant
COPY solr-8.2.0-src.tgz /usr/src/solr-8.2.0-src.tgz

RUN tar -xvf /usr/src/solr-8.2.0-src.tgz -C /usr/src
RUN cd /usr/src/solr-8.2.0 \
  && ant ivy-bootstrap \
  && ant compile \
  && cd solr \
  && ant server \
  && chmod u+x bin/solr \
  && bin/solr start -force
ENV PATH "/usr/src/solr-8.2.0/solr/bin/:${PATH}"

# search_api_solr
#  && sleep 180 \
  && sleep 5 \ 
  && solr start -force 
# =====
# final
# =====


COPY entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/entrypoint.sh

EXPOSE 80 2222
ENTRYPOINT ["entrypoint.sh"]
