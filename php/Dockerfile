FROM debian:jessie-slim
MAINTAINER Damien Carcel <damien.carcel@gmail.com>

ENV DEBIAN_FRONTEND noninteractive

# Install some useful packages
RUN apt-get update && \
    apt-get --no-install-recommends --no-install-suggests --yes --quiet install \
        apt-transport-https ca-certificates bash-completion less curl wget sudo \
        mysql-client mongodb-clients vim git imagemagick perceptualdiff && \
    apt-get clean && apt-get --yes --quiet autoremove --purge && \
    rm -rf  /var/lib/apt/lists/* /tmp/* /var/tmp/* \
            /usr/share/doc/* /usr/share/groff/* /usr/share/info/* /usr/share/linda/* \
            /usr/share/lintian/* /usr/share/locale/* /usr/share/man/*

# Add PHP 7.1 repository
RUN wget -O sury.gpg https://packages.sury.org/php/apt.gpg && apt-key add sury.gpg && rm sury.gpg
COPY files/sury.list /etc/apt/sources.list.d/sury.list

# Install and configure Gosu
RUN set -x && \
    wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/1.9/gosu-amd64" && \
    wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/1.9/gosu-amd64.asc" && \
    export GNUPGHOME="$(mktemp -d)" && \
    gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 && \
    gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu && \
    rm -r "$GNUPGHOME" /usr/local/bin/gosu.asc && \
    chmod +x /usr/local/bin/gosu && \
    gosu nobody true && \
    chown root:users /usr/local/bin/gosu && \
    chmod +s /usr/local/bin/gosu

# Install PHP with some extensions
RUN apt-get update && \
    apt-get --no-install-recommends --no-install-suggests --yes --quiet install \
        php7.1-cli php7.1-apcu php7.1-mbstring php7.1-curl php7.1-gd php7.1-imagick php7.1-intl php7.1-bcmath \
        php7.1-mcrypt php7.1-mongodb php7.1-mysql php7.1-soap php7.1-xdebug php7.1-xml php7.1-zip && \
    apt-get clean && apt-get --yes --quiet autoremove --purge && \
    rm -rf  /var/lib/apt/lists/* /tmp/* /var/tmp/* \
            /usr/share/doc/* /usr/share/groff/* /usr/share/info/* /usr/share/linda/* \
            /usr/share/lintian/* /usr/share/locale/* /usr/share/man/*

# Configure PHP CLI
RUN sed -i "s/;date.timezone =/date.timezone = Etc\/UTC/" /etc/php/7.1/cli/php.ini && \
    sed -i "s/memory_limit = .*/memory_limit = 2G/" /etc/php/7.1/cli/php.ini && \
    sed -i "s/upload_max_filesize = .*/upload_max_filesize = 20M/" /etc/php/7.1/cli/php.ini && \
    sed -i "s/post_max_size = .*/post_max_size = 20M/" /etc/php/7.1/cli/php.ini

RUN phpdismod xdebug

# Add a "docker" user
RUN useradd docker --shell /bin/bash --create-home \
  && usermod --append --groups sudo docker \
  && echo 'ALL ALL = (ALL) NOPASSWD: ALL' >> /etc/sudoers \
  && echo 'docker:secret' | chpasswd

# Install composer
RUN curl -sSL https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN chmod +x /usr/local/bin/composer

# Copy "entrypoint"
COPY ./files/entrypoint.sh /usr/local/bin
RUN chmod a+x /usr/local/bin/entrypoint.sh

# Define "docker" as current user
USER docker
WORKDIR /home/docker/

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
