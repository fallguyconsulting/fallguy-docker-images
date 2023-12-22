# The docker image we want to use
FROM php:7.2-apache

# Makes sure no questions are asked during apt-get installs
ARG DEBIAN_FRONTEND=noninteractive

# Get and install packages
RUN apt-get update --allow-releaseinfo-change \
    && apt-get -y upgrade       \
    && apt-get -y --no-install-recommends install \
        apache2-dev             \
        build-essential         \
        locate                  \
        nano                    \
        unzip                   \
        vim                     \
    && apt-get install -y zlib1g-dev libpng-dev libjpeg-dev

# moves the Apache module files from /etc/apache2/mods-available to /etc/apache2/mods-enabled
RUN a2enmod rewrite

# Install and configure GD lib
RUN docker-php-ext-configure gd --with-jpeg-dir
RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-install -j$(nproc) gettext

# Copy files/directory into the docker image
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync \
   && install-php-extensions mysqli oauth pdo_mysql

#===================================================================
# set up rpaf

ADD https://github.com/gnif/mod_rpaf/archive/stable.zip /root/rpaf/
WORKDIR /root/rpaf
RUN unzip stable.zip
WORKDIR /root/rpaf/mod_rpaf-stable

RUN make && make install

ADD config/apache/rpaf.load     /etc/apache2/mods-available/rpaf.load
ADD config/apache/rpaf.conf     /etc/apache2/mods-available/rpaf.conf

#TODO: is it nice to bake this in like so? not really. should set up a
# default and let the user configure php via a docker volume.
ADD config/php/php.ini          /usr/local/etc/php

# TODO: obviously, this is a hack for dev only. should find the right owner/group
# for this tile and set permissions appropriately instead.
RUN touch /var/log/diablo.log && chmod 777 /var/log/diablo.log
