FROM php:7.4-fpm

LABEL version="1.0"
LABEL description="Imagem php7.4-fpm costumizada."
LABEL manintainer="Marcos Pachêco"
LABEL email="marcos.hr.pacheco@gmail.com"

# define o workdir
WORKDIR /var/www

# variaveis
ENV NVM_DIR /usr/local/nvm

# criar diretorio nvm
RUN mkdir $NVM_DIR

# instala depêndencias
RUN apt-get update 
RUN apt-get install -y build-essential libpng-dev libjpeg62-turbo-dev libfreetype6-dev locales zip jpegoptim optipng pngquant gifsicle vim nano unzip git curl libzip-dev zlib1g-dev libicu-dev libgmp-dev libpq-dev libxml2-dev cron libmagickwand-dev libmagickcore-dev imagemagick
RUN pecl install imagick
RUN docker-php-ext-enable imagick
RUN docker-php-ext-enable opcache
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ 
RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql
RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-install bcmath exif pcntl zip calendar intl gmp pdo pdo_pgsql pgsql soap sockets

# Install nvm, latest node and npm
RUN curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
RUN . $NVM_DIR/nvm.sh && nvm install node && npm install npm@latest -g

# limpa o cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 

# instala o composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# adiciona usuário
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# copia os dados do diretorio atual para o workdir
COPY ./siafc /var/www/siafc
COPY ./cafor /var/www/cafor
COPY ./cafor_old /var/www/cafor_old

# copia as permissões atuais dos arquivos
COPY --chown=www:www ./siafc /var/www/siafc
COPY --chown=www:www ./cafor /var/www/cafor
COPY --chown=www:www ./cafor_old /var/www/cafor_old

# muda o usuário atual
USER www

# expõe a porta 9000 e inicia o servidor
EXPOSE 9000
CMD ["php-fpm"]
