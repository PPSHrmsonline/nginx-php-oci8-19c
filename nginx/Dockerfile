FROM nginx:mainline-alpine

# RUN set -x \
#         && adduser -D -u 1000 -g 'www-data' www-data
#         #  \
        # && chown -R core: /usr/lib/nginx

ADD ./conf.d/nginx.conf /etc/nginx/conf.d/
RUN sed -i "s/user  nginx;/user  www-data;/g" /etc/nginx/nginx.conf



        #&& curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
        #&& echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
        #&& curl -sL https://deb.nodesource.com/setup_9.x | bash - \
         