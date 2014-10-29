# OwnCloud - nginx

FROM ubuntu

MAINTAINER Norbert Schneider  <mail@herr-norbert.de>

# Install stuff
# supervisor, nginx and php dependencies
RUN apt-get update
RUN apt-get install -y supervisor nginx php5 php5-gd php-xml-parser php5-intl php5-json php5-mcrypt php5-fpm php-apc php5-imap php5-mcrypt php5-curl php5-imagick php5-pgsql php5-mysql php5-sqlite smbclient curl libcurl3 bzip2
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install OwnCloud
WORKDIR /var/www
RUN curl https://download.owncloud.org/community/owncloud-7.0.2.tar.bz2 | tar jx -C /var/www/
RUN chown -R www-data:www-data /var/www

# Configure nginx
ADD ssl.crt /etc/ssl/nginx/ssl.crt
ADD ssl.key /etc/ssl/nginx/ssl.key
ADD nginx.conf /etc/nginx/nginx.conf

# Configure php
RUN sed -i -e "s/^upload_max_filesize\s*=\s*2M/upload_max_filesize = 16G/" /etc/php5/fpm/php.ini
RUN sed -i -e "s/^post_max_size\s*=\s*8M/post_max_size = 16G/" /etc/php5/fpm/php.ini
RUN sed -i -e "s/^output_buffering\s*=\s*4096/output_buffering = 0/" /etc/php5/fpm/php.ini

# Configure  php-fpm
ADD www.conf /etc/php5/fpm/pool.d/www.conf

# Configure supervisord
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

EXPOSE 443

# Start everything
CMD supervisord
