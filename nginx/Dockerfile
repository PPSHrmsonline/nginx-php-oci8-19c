FROM nginx:mainline-alpine

RUN set -x \
        && adduser -D -u 1000 -g 'pps' pps
        #  \
        # && chown -R core: /usr/lib/nginx

RUN sed -i "s/user  nginx;/user  core;/g" /etc/nginx/nginx.conf
