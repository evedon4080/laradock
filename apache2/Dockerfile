FROM webdevops/apache:ubuntu-18.04

LABEL maintainer="Eric Pfeiffer <computerfr33k@users.noreply.github.com>"

ARG PHP_UPSTREAM_CONTAINER=php-fpm
ARG PHP_UPSTREAM_PORT=9000
ARG PHP_UPSTREAM_TIMEOUT=60
ARG DOCUMENT_ROOT=/var/www/

ENV WEB_PHP_SOCKET=${PHP_UPSTREAM_CONTAINER}:${PHP_UPSTREAM_PORT}

ENV WEB_DOCUMENT_ROOT=${DOCUMENT_ROOT}

ENV WEB_PHP_TIMEOUT=${PHP_UPSTREAM_TIMEOUT}

EXPOSE 80 443

WORKDIR /var/www/

COPY vhost.conf /etc/apache2/sites-enabled/vhost.conf

# SSL証明書をコピー
COPY ssl/app-burdock.docker.pem /etc/apache2/ssl/app-burdock.docker.pem
COPY ssl/app-burdock.docker-key.pem /etc/apache2/ssl/app-burdock.docker-key.pem
COPY ssl/_wildcard.app-burdock-preview.docker+1.pem /etc/apache2/ssl/_wildcard.app-burdock-preview.docker+1.pem
COPY ssl/_wildcard.app-burdock-preview.docker+1-key.pem /etc/apache2/ssl/_wildcard.app-burdock-preview.docker+1-key.pem

# mod_rewriteを 有効に
RUN a2enmod rewrite
RUN ln -s /etc/apache2/mods-available/vhost_alias.load /etc/apache2/mods-enabled
RUN echo ServerName $HOSTNAME > /etc/apache2/conf-available/fqdn.conf && a2enconf fqdn
RUN a2enmod headers
RUN a2enmod include

ENTRYPOINT ["/opt/docker/bin/entrypoint.sh"]

CMD ["supervisord"]
