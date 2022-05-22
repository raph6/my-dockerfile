FROM php:8.1.4-fpm-alpine3.15

ARG TZ='Europe/Paris'
ENV DEFAULT_TZ ${TZ}

RUN apk --update --no-cache --no-progress add tzdata && \
    cp /usr/share/zoneinfo/${DEFAULT_TZ} /etc/localtime

# RUN apk add --update --no-cache --no-progress \
# 		curl-dev \
#         musl-dev \
# 		libcurl \
# 		libzip-dev \
# 		bzip2-dev \
# 		openssl-dev \
# 		git \
# 		libressl-dev \
# 		ca-certificates \
# 		autoconf \
# 		make \
# 		gcc \
# 		linux-headers \
# 	&& docker-php-ext-install \
# 	    pdo_mysql zip opcache

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Use the default production configuration
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"
RUN sed -i -e 's/expose_php = On/expose_php = Off/' "$PHP_INI_DIR/php.ini"
RUN sed -i -e 's/disable_functions =/disable_functions =exec,passthru,shell_exec,system,proc_open,popen,curl_exec,curl_multi_exec,parse_ini_file,show_source/' "$PHP_INI_DIR/php.ini"
RUN sed -i -e 's/allow_url_fopen = On/allow_url_fopen = Off/' "$PHP_INI_DIR/php.ini"

WORKDIR /var/www/html

ADD . .
RUN chmod 777 -R tmp
CMD [ "php-fpm" ]