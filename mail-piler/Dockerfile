FROM ubuntu:18.04
ENV DEBIAN_FRONTEND noninteractive
ENV PILER_USER piler
ENV PILER_DEB https://bitbucket.org/jsuto/piler/downloads/piler_1.3.5~bionic-f2e4cb1_amd64.deb

RUN \
# Update and get dependencies
    apt-get update \
	&& apt-get -y dist-upgrade \
	&& apt-get -y install \
        curl \
        nginx \
        procps \
        cron \
        mysql-client \
        libtre-dev \
        default-libmysqlclient-dev \
        libssl-dev \ 
        libwrap0-dev \ 
        libzip-dev \
        catdoc \
        php-curl \
        php-fpm \
        php-gd \
        php-ldap \
        php-memcache \
        php-mysql \ 
        php-pdo \
        poppler-utils \ 
        python-mysqldb \
        sphinxsearch \ 
        sysstat \ 
        tnef \ 
        unrtf \
        libmariadb-dev \
        supervisor \
        inetutils-syslogd \
    && \  
# Install Piler
    curl -J -L -o /tmp/piler.deb "https://bitbucket.org/jsuto/piler/downloads/piler_1.3.5~bionic-f2e4cb1_amd64.deb" && \
    dpkg -i /tmp/piler.deb && \
# Cleanup
    apt-get -y autoremove && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/* && \
    rm -rf /var/tmp/*

COPY rootfs /
RUN \
# Move Files
    mv /tmp/startup.sh /usr/share/piler/startup.sh && \
    mv /tmp/wait.sh /usr/share/piler/wait.sh && \
    mv /tmp/db-mysql.sql /usr/share/piler/db-mysql.sql && \
# Change permissions
    chmod +x /usr/share/piler/startup.sh &&\
    chmod +x /usr/share/piler/wait.sh 
RUN crontab /tmp/piler.cron
EXPOSE 25/tcp
EXPOSE 80/tcp

ENTRYPOINT ["/usr/share/piler/startup.sh"]
VOLUME /var/piler/store/00 /var/piler/sphinx