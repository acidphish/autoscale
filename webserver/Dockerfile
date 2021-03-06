FROM ubuntu:latest

MAINTAINER Nikolay Damianov <ndmaianov@gmail.com>

# Install Apache, PHP and MySQL into image
RUN apt-get update && apt-get -y upgrade && DEBIAN_FRONTEND=noninteractive apt-get -y install \
    apache2 php7.0 php7.0-mysql libapache2-mod-php7.0 curl lynx-cur

# Enable Apache modules
RUN a2enmod php7.0
RUN a2enmod rewrite
RUN a2enmod headers

# Enable short PHP tags (<? and ?>)
RUN sed -i "s/short_open_tag = Off/short_open_tag = On/" /etc/php/7.0/apache2/php.ini
RUN sed -i "s/error_reporting = .*$/error_reporting = E_ERROR | E_WARNING | E_PARSE/" /etc/php/7.0/apache2/php.ini

# Setup Apache environment variables
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid

# Listen on port 80
EXPOSE 80

# Default website configuration
ADD apache-config.conf /etc/apache2/sites-enabled/000-default.conf

# Add the default website
ADD html /var/www/autoscale

# Start Apache in the foreground
CMD /usr/sbin/apache2ctl -D FOREGROUND

