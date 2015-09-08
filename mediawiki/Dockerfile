from debian:jessie

env DEBIAN_FRONTEND noninteractive
RUN apt-get update
RUN apt-get install -y php5-mcrypt php5-sqlite php5-mysql sqlite php5-gd php5-intl imagemagick pwgen mysql-client wget \
    supervisor nginx-light php5-fpm php5-cli curl procps vim git bzip2


ENV MEDIAWIKI_VERSION_MAJOR 1.22
ENV MEDIAWIKI_VERSION_MINOR 15
RUN wget http://releases.wikimedia.org/mediawiki/${MEDIAWIKI_VERSION_MAJOR}/mediawiki-${MEDIAWIKI_VERSION_MAJOR}.${MEDIAWIKI_VERSION_MINOR}.tar.gz && \
 tar xzf mediawiki-${MEDIAWIKI_VERSION_MAJOR}.${MEDIAWIKI_VERSION_MINOR}.tar.gz -C /srv
# or much faster add mediawiki-${MEDIAWIKI_VERSION_MAJOR}.${MEDIAWIKI_VERSION_MINOR}.tar.gz /srv/
# RUN cd /srv && tar xzf mediawiki-${MEDIAWIKI_VERSION_MAJOR}.${MEDIAWIKI_VERSION_MINOR}.tar.gz -C /srv
RUN mv /srv/mediawiki-${MEDIAWIKI_VERSION_MAJOR}.${MEDIAWIKI_VERSION_MINOR} /srv/mediawiki
RUN chown -R www-data /srv/mediawiki/mw-config
RUN ln -sf /srv/mediawiki/config/LocalSettings.php /srv/mediawiki/LocalSettings.php

ADD assets/add_extension.sh /usr/local/bin/add_extension.sh
RUN mkdir -p /srv/mediawiki/downloaded_extensions

# ADD https://extdist.wmflabs.org/dist/extensions/BlockAndNuke-REL1_25-f01f9b7.tar.gz      /srv/mediawiki/downloaded_extensions
# ADD https://extdist.wmflabs.org/dist/extensions/Collection-REL1_25-d898236.tar.gz        /srv/mediawiki/downloaded_extensions
# ADD https://extdist.wmflabs.org/dist/extensions/ConfirmEdit-REL1_25-48cc81c.tar.gz       /srv/mediawiki/downloaded_extensions
# ADD https://extdist.wmflabs.org/dist/extensions/LanguageSelector-REL1_25-d230e55.tar.gz  /srv/mediawiki/downloaded_extensions
# ADD https://extdist.wmflabs.org/dist/extensions/Nuke-REL1_25-e18b127.tar.gz              /srv/mediawiki/downloaded_extensions
# ADD https://extdist.wmflabs.org/dist/extensions/Vector-REL1_25-8c5d979.tar.gz            /srv/mediawiki/downloaded_extensions
# ADD https://extdist.wmflabs.org/dist/extensions/WikiEditor-REL1_25-17b31b5.tar.gz        /srv/mediawiki/downloaded_extensions
# RUN /usr/local/bin/add_extension.sh BlockandNuke
# RUN /usr/local/bin/add_extension.sh Collection
# RUN /usr/local/bin/add_extension.sh ConfirmEdit
# RUN /usr/local/bin/add_extension.sh LanguageSelector
# RUN /usr/local/bin/add_extension.sh Nuke
# RUN /usr/local/bin/add_extension.sh Vector
# RUN /usr/local/bin/add_extension.sh WikiEditor


# I download manually these files to my downloaded_extensions as I still need to play around a lot to find
# the best working combination
# The next steps also unpacks all tar.gz files!
ADD downloaded_extensions/* /srv/mediawiki/extensions/

ADD site.conf /etc/nginx/sites-enabled/default
ADD assets/start_mediawiki /usr/local/bin/start_mediawiki
RUN echo 'cgi.fix_pathinfo=0' >>/etc/php5/fpm/php.ini
ADD supervisor.conf /etc/supervisor/conf.d/nginx.conf
ADD ./fpm.conf /etc/php5/fpm/php-fpm.conf
ADD ./fpm-pool-www.conf /etc/php5/fpm/pool.d/www.conf
volume /srv/mediawiki/images
volume /srv/mediawiki/config
volume /srv/www

expose 80
env HOME=/root

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

entrypoint ["/usr/local/bin/start_mediawiki"]
cmd ["/usr/bin/supervisord", "--nodaemon"]
