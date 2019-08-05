FROM ubuntu:18.04
ENV DEBIAN_FRONTEND noninteractive
ENV PILER_USER piler
ENV SPHINX_BIN_TARGZ sphinx-3.1.1-bin.tar.gz
ENV PILER_HOSTNAME localhost
ENV PILER_DEB https://bitbucket.org/jsuto/piler/downloads/piler_1.3.5~bionic-f2e4cb1_amd64.deb
ENV SPHINX_DOWNLOAD_URL https://sphinxsearch.com/files/sphinx-3.1.1-612d99f-linux-amd64.tar.gz

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
        sysstat \ 
        tnef \ 
        unrtf \
        libmariadb-dev \
        supervisor \
        inetutils-syslogd \
        wget \
    && \ 
# Install Sphinxsearch
    wget --no-check-certificate -q -O ${SPHINX_BIN_TARGZ} ${SPHINX_DOWNLOAD_URL} && \
    tar zxvf ${SPHINX_BIN_TARGZ} && \
    rm -f ${SPHINX_BIN_TARGZ} && \
    cp sphinx-3.1.1/bin/* /usr/bin/ && \
# Cleanup
    apt-get -y autoremove && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/* && \
    rm -rf /var/tmp/*

COPY rootfs /
RUN \
# Make directory
    mkdir /run/php/ && \
    mkdir /usr/share/piler && \
# Move files
    mv /tmp/startup.sh /usr/share/piler/startup.sh && \
    mv /tmp/wait.sh /usr/share/piler/wait.sh && \
    mv /tmp/db-mysql.sql /usr/share/piler/db-mysql.sql && \
# Change permissions
    chmod +x /usr/share/piler/startup.sh &&\
    chmod +x /usr/share/piler/wait.sh 
EXPOSE 25/tcp
EXPOSE 80/tcp
EXPOSE 443/tcp

ENTRYPOINT ["/usr/share/piler/startup.sh"]
VOLUME /var/piler/store/00 /var/piler/sphinx /etc/piler