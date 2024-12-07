FROM php:8.2-fpm

# Declare the base Composer version
ENV COMPOSER_ROOT_VERSION='2.8.3'

# Install Composer
COPY --from=composer/composer:latest-bin /composer /usr/bin/composer

# PHP Core and WordPress extensions
RUN apt-get update && apt-get install -yq --no-install-recommends \
		libexif-doc \
		libfreetype-dev \
		libjpeg62-turbo-dev \
		libpng-dev \
		libicu-dev \
		libmagickwand-dev \
		zip libzip-dev \
		default-libmysqlclient-dev \
		default-mysql-client \
	&& docker-php-ext-configure gd --with-freetype --with-jpeg \
	&& docker-php-ext-install -j$(nproc) gd mysqli pdo pdo_mysql zip \
	&& docker-php-ext-install exif \
	&& docker-php-ext-install intl \
	&& pecl install imagick \
	&& docker-php-ext-enable imagick \
	&& mkdir -p /.config/composer \
		/.composer \
		/.cache \
	# Install WP-CLI
	&& curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
	&& chmod +x wp-cli.phar \
	&& mv wp-cli.phar /usr/local/bin/wp

	# Copy the Composer keys
	COPY keys.dev.pub /.config/composer
	COPY keys.tags.pub /.config/composer
	
	# Claim ownership for the www-data user
	RUN chown -R www-data:www-data /.cache /.composer /.config