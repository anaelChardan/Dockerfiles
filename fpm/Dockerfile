FROM carcel/php:5.6
MAINTAINER Damien Carcel <damien.carcel@gmail.com>

# Define "root" as current user
USER root

# Install PHP FPM
RUN apt-get update && \
    apt-get -yq install php5-fpm && \
    apt-get clean && apt-get -yq autoclean && apt-get -yq autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Configure PHP FPM
RUN sed -i "s/;date.timezone =/date.timezone = Europe\/Paris/" /etc/php5/fpm/php.ini && \
    sed -i "s/memory_limit = .*/memory_limit = 512M/" /etc/php5/fpm/php.ini

RUN sed -i "s/user = www-data/user = docker/" /etc/php5/fpm/pool.d/www.conf && \
    sed -i "s/group = www-data/group = docker/" /etc/php5/fpm/pool.d/www.conf && \
    sed -i "s/listen.owner = www-data/listen.owner = docker/" /etc/php5/fpm/pool.d/www.conf && \
    sed -i "s/listen.group = www-data/listen.group = docker/" /etc/php5/fpm/pool.d/www.conf

# Expose PHP FPM to the host
EXPOSE 9000

# Define "docker" as current user
USER docker

# Run apache in foreground
CMD ["sudo", "php5-fpm", "-F"]