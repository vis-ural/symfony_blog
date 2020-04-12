FROM php:7.4-fpm
LABEL MAINTAINER="ilogachev@infomarketstudio.ru "
# Arguments defined in docker-compose.yml
ARG user
ARG uid

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    nano \
    apt-utils \
    iputils-ping


# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd


#RUN pecl install mongodb \
#    &&  echo "extension=mongo.so" > /usr/local/etc/php/conf.d/mongo.ini

RUN pecl install -o -f xdebug \
    && rm -rf /tmp/pear

# Set timezone
RUN rm /etc/localtime
RUN ln -s /usr/share/zoneinfo/Europe/Paris /etc/localtime
RUN "date"

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user
RUN mkdir -p /var/lib/php/session/
RUN chmod -R 777 /var/lib/php/session/

#copy php & fpm settings
COPY ./php.ini /usr/local/etc/php/
COPY ./www.conf /usr/local/etc/php-fpm.d/www.conf
# Set working directory
WORKDIR /var/www/blog

USER $user

EXPOSE 9000

CMD ["php-fpm"]
# copy CMD script
#COPY ./php_cmd.sh /usr/bin/php_cmd.sh
#RUN chmod +x /usr/bin/php_cmd.sh
#CMD ["php_cmd.sh"]

