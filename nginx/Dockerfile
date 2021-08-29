ARG PHP_VERSION="8.0"
FROM craftcms/php-fpm:${PHP_VERSION}

ARG NGINX_CONF=default.conf

# install supervisor and nginx
USER root
RUN apk update --no-cache && apk add --no-cache supervisor nginx git openssh
COPY supervisor.conf /etc/supervisor/conf.d/supervisor.conf
COPY nginx.conf /etc/nginx/nginx.conf
COPY craftcms/general.conf /etc/nginx/craftcms/
COPY craftcms/php_fastcgi.conf /etc/nginx/craftcms/
COPY craftcms/security.conf /etc/nginx/craftcms/
COPY ${NGINX_CONF} /etc/nginx/conf.d/default.conf
RUN touch /run/nginx.pid
RUN touch /run/supervisord.pid
RUN chown www-data /run/nginx.pid
RUN chown www-data /run/supervisord.pid
RUN chown -R www-data:www-data /var/lib/nginx/logs/
RUN chown -R www-data:www-data /var/lib/nginx
# SSH key config
RUN mkdir /home/www-data/.ssh
RUN ln -s /etc/keyfiles/deploy-key /home/www-data/.ssh/id_rsa
RUN ln -s /etc/keyfiles/deploy-key.pub /home/www-data/.ssh/id_rsa.pub
RUN echo "github.com ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAq2A7hRGmdnm9tUDbO9IDSwBK6TbQa+PXYPCPy6rbTrTtw7PHkccKrpp0yVhp5HdEIcKr6pLlVDBfOLX9QUsyCOV0wzfjIJNlGEYsdlLJizHhbn2mUjvSAHQqZETYP81eFzLQNnPHt4EVVUh7VfDESU84KezmD5QlWpXLmvU31/yMf+Se8xhHTvKSCZIFImWwoG6mbUoWf9nzpIoaSjB+weqqUUmpaaasXVal72J+UX2B+2RPW3RcT0eOzQgqlJL3RKrTJvdsjE3JEAvGq3lGHSZXy28G3skua2SmVi/w4yCE6gbODqnTWlg7+wC604ydGXA8VJiS5ap43JXiUFFAaQ==" > /home/www-data/.ssh/known_hosts
RUN chown -R www-data:www-data /home/www-data/.ssh



USER www-data

EXPOSE 8080
ENTRYPOINT ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisor.conf"]

# ONBUILD COPY --chown=www-data:www-data . .
