FROM ubuntu:18.04
ENV DEBIAN_FRONTEND noninteractive
ENV PILER_USER piler
ENV SPHINX_BIN_TARGZ sphinx-3.3.1-bin.tar.gz
ENV PILER_HOSTNAME localhost
ENV PILER_DEB https://bitbucket.org/jsuto/piler/downloads/piler_1.3.11-focal-5c2ceb1_amd64.deb
ENV SPHINX_DOWNLOAD_URL hhttps://sphinxsearch.com/files/sphinx-3.3.1-b72d67b-linux-amd64.tar.gz

RUN \
# Update and get dependencies
    apt-get update \
	&& apt-get -y dist-upgrade \
	&& apt-get -y install \
        openssl \ 
        php7.2-cli \
        php7.2-cgi \ 
        php7.2-mysql \ 
        php7.2-fpm \ 
        php7.2-zip \ 
        php7.2-ldap \
        php7.2-gd \ 
        php7.2-curl \ 
        php7.2-xml \ 
        php7.2-memcache \
        php7.2-pdo \
        sudo \
        libodbc1 \
        libpq5 \
        libzip4 \
        libtre5 \
        libwrap0 \
        libmariadb3 \ 
        python \
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
    mv /tmp/config-site.php /usr/share/piler/config-site.php && \
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
