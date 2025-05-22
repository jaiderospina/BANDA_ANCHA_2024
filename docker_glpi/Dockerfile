FROM php:7.4-apache

ENV GLPI_VERSION=9.5.11
ENV TIMEZONE=America/Bogota

# Mejoras para entornos con proxy
RUN echo "Acquire::http::Pipeline-Depth 0;" > /etc/apt/apt.conf.d/99custom && \
    echo "Acquire::http::No-Cache true;" >> /etc/apt/apt.conf.d/99custom && \
    echo "Acquire::BrokenProxy true;" >> /etc/apt/apt.conf.d/99custom

# Instala dependencias del sistema y extensiones necesarias
RUN apt-get update && apt-get install -y --no-install-recommends \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    libicu-dev \
    libldap2-dev \
    libbz2-dev \
    libargon2-dev \
    unzip \
    zip \
    git \
    curl \
    mariadb-client \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) \
        gd \
        pdo \
        pdo_mysql \
        mbstring \
        exif \
        pcntl \
        xml \
        zip \
        intl \
        opcache \
        bcmath \
        bz2 \
        mysqli \
        ldap \
        xmlrpc \
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Configura zona horaria
RUN ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone

# Descarga GLPI y lo instala
RUN mkdir -p /var/www/html/glpi \
    && curl -L https://github.com/glpi-project/glpi/releases/download/$GLPI_VERSION/glpi-$GLPI_VERSION.tgz \
    | tar -xz --strip-components=1 -C /var/www/html/glpi

# Cambia permisos para Apache
RUN chown -R www-data:www-data /var/www/html/glpi && chmod -R 755 /var/www/html/glpi

# Habilita .htaccess y módulos necesarios de Apache
RUN a2enmod rewrite

# Agrega configuración de Apache para permitir acceso al directorio
RUN echo '<Directory /var/www/html>\n\
    Options Indexes FollowSymLinks\n\
    AllowOverride All\n\
    Require all granted\n\
</Directory>' >> /etc/apache2/apache2.conf

# Copia configuración personalizada de PHP si la tienes
COPY config/php.ini /usr/local/etc/php/

# Expone el puerto
EXPOSE 80
