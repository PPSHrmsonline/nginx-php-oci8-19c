FROM nginx
 
RUN groupadd ppshrms
RUN useradd -g ppshrms -d /var/www/html -s /sbin/nologin ppshrms
RUN mkdir -p /etc/nginx/sites-enabled/ 
ADD conf.d/hcm.conf /etc/nginx/sites-available/
RUN  ln -s /etc/nginx/sites-available/hcm.conf /etc/nginx/sites-enabled/hcm.conf


        #&& curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
        #&& echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
        #&& curl -sL https://deb.nodesource.com/setup_9.x | bash - \
         