FROM php:7.4-fpm-buster
MAINTAINER Kevin Williams (@llslim) <info@llslim.com>

RUN set -eux; \
\
	if command -v a2enmod; then \
		a2enmod rewrite; \
	fi; \
	buildDeps=" \
		libfreetype6-dev \
		libjpeg-dev \
		libpng-dev \
		libpq-dev \
		libzip-dev \
	 "; \
	 supportServices=" \
	       msmtp \
	       msmtp-mta \
	       gdb \
	 "; \
	  apt-get update; \
	  export DEBIAN_FRONTEND=noninteractive \
	  && apt-get install -y --no-install-recommends $supportServices; \
	  savedAptMark="$(apt-mark showmanual)"; \
	  apt-get install -y --no-install-recommends $buildDeps; \
	 # build php extensions with development dependencies, and install them
	 docker-php-ext-configure \
	   gd --with-freetype --with-jpeg=/usr; \
	 docker-php-ext-install -j "$(nproc)" gd opcache pdo pdo_mysql pdo_pgsql mysqli zip; \

	# reset apt-mark's "manual" list so that "purge --auto-remove" will remove all build dependencies
	 apt-mark auto '.*' > /dev/null; \
	 apt-mark manual $savedAptMark; \
	 ldd "$(php -r 'echo ini_get("extension_dir");')"/*.so \
	   | awk '/=>/ { print $3 }' \
	   | sort -u \
	   | xargs -r dpkg-query -S \
	   | cut -d: -f1 \
	   | sort -u \
	   | xargs -rt apt-mark manual; \
	 \
	 apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
	  rm -rf /var/lib/apt/lists/*; \
	# install xdebug extension
	pear config-set php_ini /usr/local/etc/php/php.ini; \
	pecl install xdebug; \
 	docker-php-ext-enable xdebug

# copy default php.ini into image
COPY php.ini /usr/local/etc/php/

# copy the php config files for modules into image
COPY ./php-conf.d/*.ini /usr/local/etc/php/conf.d/

# copy the php fpm files into image 
COPY ./php-fpm.d/* /usr/local/etc/php-fpm.d/

#	 openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
#	  -keyout /etc/ssl/private/ssl-cert-llslim.key -out /etc/ssl/certs/ssl-cert-llslim.pem \
#		 -subj "/C=US/ST=NC/L=Vien/O=Security/OU=Development/CN=localhost"

	 COPY ./msmtprc /etc/msmtprc

	 EXPOSE 443
	 WORKDIR /var/www/html/
