FROM php:8.1-fpm-alpine

# Fetch arguments from build command
ARG SHOPIFY_API_KEY
ENV SHOPIFY_API_KEY=${SHOPIFY_API_KEY}
ARG PORT
ENV PORT=${PORT}

# Install dependent packages
RUN apk update && apk add --update nodejs npm \
    libzip-dev \
    curl-dev \
    libxml2-dev \
    postgresql-dev \
    openrc \
    bash \
    nginx

# Compile needed extensions
RUN docker-php-ext-install pdo zip pdo_mysql mysqli curl fileinfo pdo_pgsql simplexml dom

# Copy application level
COPY --chown=www-data:www-data web /app
WORKDIR /app

# Overwrite default nginx config
COPY web/nginx.conf /etc/nginx/nginx.conf

# Use the default production configuration
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

# Enable extensions in php.ini
#RUN echo "extension=zip.so" >> $PHP_INI_DIR/php.ini

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer
RUN composer install

# Compile application
RUN cd frontend \
    && npm install \
    && npm run build --api-key=8a9d253ee9bf73241927d8d5aad063ba

# Composer
RUN composer build && composer update

# Fix storage/ dir ownership
RUN chown www-data:www-data /app/storage /app/vendor -R

EXPOSE 80

ENTRYPOINT [ "/app/entrypoint.sh" ]
