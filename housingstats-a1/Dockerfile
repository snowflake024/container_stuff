FROM php:5.6-apache

# Update and install php dependancies
RUN apt-get update && apt-get -y upgrade && apt-get install -y \
    apt-utils \
    #GD dep
    libpng-dev \ 
    zlib1g-dev \
    #GMP dep
    libgmp-dev \
    gawk

# Configure apache
RUN sed -i "s/DocumentRoot .*/DocumentRoot \/var\/www\/html\/public/" /etc/apache2/apache2.conf
RUN echo "error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT" >> /usr/local/etc/php/conf.d/error.ini
RUN echo "log_errors = On" >> /usr/local/etc/php/conf.d/error.ini
RUN a2enmod rewrite

# Prerequisite for debian systems for php-gmp module
RUN ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h

# Install php modules

RUN docker-php-ext-install mbstring exif gd gettext gmp mysql mysqli pdo_mysql shmop

# Not really needed

#RUN echo "extension=mbstring.so" >> /usr/local/etc/php/php.ini

#RUN echo "extension=exif.so" >> /usr/local/etc/php/php.ini

#RUN echo "extension=gd.so" >> /usr/local/etc/php/php.ini    

#RUN echo "extension=gettext.so" >> /usr/local/etc/php/php.ini  

#RUN echo "extension=gmp.so" >> /usr/local/etc/php/php.ini  

#RUN echo "extension=mysql.so" >> /usr/local/etc/php/php.ini  

#RUN echo "extension=mysqli.so" >> /usr/local/etc/php/php.ini

#RUN echo "extension=pdo_mysql.so" >> /usr/local/etc/php/php.ini  

#RUN echo "extension=shmop.so" >> /usr/local/etc/php/php.ini 


# Fetch php.ini
RUN mv /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini
