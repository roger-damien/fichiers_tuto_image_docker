# Dockerfile
FROM php:8.2-apache

# Variables temporaire (ici juste pour l'exemple, utilisation dans apt-get install)
ARG APT_FLAGS="-q -y"

# Variables d'environnements. Depuis la version 2.4.2, masque l'avertissement Composer pour les commandes exécutées en tant que super-utilisateur
ENV COMPOSER_ALLOW_SUPERUSER=1

# Applications pour Composer 
RUN apt-get update -qq && \
    apt-get install ${APT_FLAGS} \
    git \
    gnupg \
    unzip \
    zip && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Extensions PHP gd, pdo_mysql, bcmath, opcacheet et copie du fichier ini additionnel
# La commande Linux nproc donne directement au script le nombre de threads physiques disponibles pour votre système.
RUN apt-get install ${APT_FLAGS} \
    libfreetype6-dev \
	libjpeg62-turbo-dev \
	libpng-dev \
	&& docker-php-ext-configure gd --with-freetype --with-jpeg \
	&& docker-php-ext-install -j$(nproc) gd pdo_mysql bcmath opcache && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Déplacement des fichiers de configuration de PHP et d'Apache, et copie du fichier index.php dans le workdir
COPY php.ini /usr/local/etc/php/conf.d/user.ini
COPY vhost.conf /etc/apache2/sites-available/000-default.conf
COPY apache.conf /etc/apache2/conf-available/monsite.conf
COPY index.php /var/www/html/index.php

# Désignation du répertoire du travail
WORKDIR /var/www/html   

# Ouverture du port http
EXPOSE 80