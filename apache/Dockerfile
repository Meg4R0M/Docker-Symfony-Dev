FROM ubuntu:latest
MAINTAINER Florian Durano <psykoterro@gmail.com>

# Set apt-get to automatically retry if a package download fails
RUN echo 'Acquire::Retries "5";' > /etc/apt/apt.conf.d/99AcquireRetries

# Update and upgrade image, remove all unused package
RUN apt-get update && apt-get -y dist-upgrade && apt-get -y autoremove

# Install apache2
RUN apt-get install -y \
    curl git \
    apache2 apache2-utils libapache2-mod-php \
    apache2-suexec-pristine mcrypt \
    imagemagick libruby libapache2-mod-python \
    memcached php-memcache php-imagick php-gettext php7.0-mysql

RUN apt-get clean && rm -rf /var/lib/apt/lists/*
RUN /usr/sbin/a2dismod 'mpm_*' && /usr/sbin/a2enmod mpm_prefork

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2

RUN /usr/sbin/a2ensite default-ssl
RUN /usr/sbin/a2enmod ssl

EXPOSE 80
EXPOSE 443

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
