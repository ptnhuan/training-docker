FROM php:8.3.7-apache

# Declare composer version
ENV COMPOSER_VERSION=2.4.4

RUN apt-get update \
    && apt-get install -y \
        curl \
        zip \
        unzip \
        git \
        supervisor \
        libpng-dev \
        libpq-dev \
        libzip-dev \
        gnupg \
        tzdata \
    && docker-php-ext-install \
        bcmath \
        gd \
        opcache \
        sockets \
        zip \
        mysqli \
        pdo_mysql

RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && curl https://packages.microsoft.com/config/debian/11/prod.list > /etc/apt/sources.list.d/mssql-release.list
RUN apt-get update \
    && ACCEPT_EULA=Y apt-get install -y \
        msodbcsql18 \
        mssql-tools18 \
    && apt-get install -y \
        --allow-downgrades odbcinst=2.3.7 odbcinst1debian2=2.3.7 unixodbc=2.3.7 unixodbc-dev=2.3.7 \
        libgssapi-krb5-2 \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN echo 'export PATH="$PATH:/opt/mssql-tools18/bin"' >> ~/.bashrc
RUN . ~/.bashrc
RUN pecl install sqlsrv \
    && pecl install pdo_sqlsrv\
    && docker-php-ext-enable \
     sqlsrv \
     pdo_sqlsrv

RUN sed -ir -e '1i [ODBC]\nPooling=Yes\n' /etc/odbcinst.ini
RUN sed -ir -e '$a CPTimeout=120' /etc/odbcinst.ini

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

ENV TZ Asia/Tokyo

ENV APACHE_DOCUMENT_ROOT /var/www/html/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf

# Install Composer with specified version
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version=$COMPOSER_VERSION

RUN a2enmod rewrite

RUN useradd -ms /bin/bash -u 1000 app

COPY start /usr/local/bin/

#CMD [ "start" ]