FROM tasktrack.telekom.at/remote-docker/php:5.6-apache

# Some arguments
ARG http_proxy=http://proxy.austria.local:8080
ARG https_proxy=http://proxy.austria.local:8080
ARG no_proxy=127.0.0.1,localhost,10.*,185.*,*.pta.at,*.inside,*.local,*.telekom.at,*.a1telekom.at,tasktrack.telekom.at

# Update and install php dependancies
RUN apt-get update && apt-get -y upgrade && apt-get install -y \
    apt-utils \
    #GD dep
    libpng-dev \
    zlib1g-dev \
    #GMP dep
    libgmp-dev \
    gawk \
    telnet \
    bash \
    vim

# Configure apache
RUN sed -i "s/DocumentRoot .*/DocumentRoot \/var\/www\/html\/public/" /etc/apache2/apache2.conf
RUN echo "ServerName housingstats-new.a1telekom.inside" >> /etc/apache2/apache2.conf
RUN echo "error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT" >> /usr/local/etc/php/conf.d/error.ini
RUN echo "log_errors = On" >> /usr/local/etc/php/conf.d/error.ini

RUN a2enmod rewrite

# Fix file permissions in /var/log/html\ directory

RUN chown -R www-data:www-data /var/www/html

# Prerequisite for debian systems for php-gmp module
RUN ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h

# Fetch php.ini
RUN mv /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini

# Install php modules
RUN docker-php-ext-install mbstring exif gd gettext gmp mysql mysqli pdo_mysql shmop

# Modify php.ini with needed settings

RUN sed -i "s/allow_url_include/#allow_url_include/" /usr/local/etc/php/php.ini


RUN echo "allow_url_include = On" >> /usr/local/etc/php/php.ini

# Not really needed but good to have
#RUN echo "extension=mbstring.so" >> /usr/local/etc/php/php.ini

#RUN echo "extension=exif.so" >> /usr/local/etc/php/php.ini

#RUN echo "extension=gd.so" >> /usr/local/etc/php/php.ini

#RUN echo "extension=gettext.so" >> /usr/local/etc/php/php.ini

#RUN echo "extension=gmp.so" >> /usr/local/etc/php/php.ini

#RUN echo "extension=mysql.so" >> /usr/local/etc/php/php.ini

#RUN echo "extension=mysqli.so" >> /usr/local/etc/php/php.ini

#RUN echo "extension=pdo_mysql.so" >> /usr/local/etc/php/php.ini

#RUN echo "extension=shmop.so" >> /usr/local/etc/php/php.ini
### 

# Fix deprecated commenting of lines in php.ini

RUN find /usr/local/etc/php/ -name "*.ini" -exec sed -i -re 's/^(\s*)#(.*)/\1;\2/g' {} \;

