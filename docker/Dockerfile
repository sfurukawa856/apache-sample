# FROM php:7.0-apache
FROM php:8.3-apache

RUN groupadd -g 1000 apache && useradd -u 1000 -g apache apache

COPY ./src /var/www/html/apache-sample

RUN chown -R apache:apache /var/www/html/apache-sample

COPY ./docker/apache2.conf /etc/apache2/apache2.conf
COPY ./docker/000-default.conf /etc/apache2/sites-available/000-default.conf

RUN a2enmod rewrite

RUN echo "deb http://archive.debian.org/debian stretch main contrib non-free" > /etc/apt/sources.list \
	&& echo "deb http://archive.debian.org/debian-security stretch/updates main contrib non-free" >> /etc/apt/sources.list \
	&& apt-get update \
	&& apt-get install -y \
	git \
	zip \
	unzip \
	&& php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
	&& php composer-setup.php \
	&& mv composer.phar /usr/local/bin/composer \
	&& php -r "unlink('composer-setup.php');" \
	&& docker-php-ext-install pdo_mysql

WORKDIR /var/www/html/apache-sample

CMD ["apache2-foreground"]