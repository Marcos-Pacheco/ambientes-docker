FROM php:7.2-fpm

LABEL version="1.0"
LABEL description="Imagem php7.2-fpm costumizada."
LABEL manintainer="Marcos Pachêco"
LABEL email = "marcos.hr.pacheco@gmail.com"

WORKDIR /var/www

RUN apt-get update 
RUN apt-get install -y build-essential libpng-dev libjpeg62-turbo-dev libfreetype6-dev locales zip jpegoptim optipng pngquant gifsicle vim nano unzip git curl libzip-dev zlib1g-dev libicu-dev libgmp-dev libpq-dev libxml2-dev cron
RUN docker-php-ext-enable opcache
#RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql
#RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-install bcmath exif pcntl zip calendar intl gmp pdo pdo_pgsql pgsql soap sockets

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN groupadd -g 1000 www

RUN useradd -u 1000 -ms /bin/bash -g www www

USER $user

EXPOSE 9000

CMD ["php-fpm"]
