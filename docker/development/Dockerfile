FROM php:8.2-cli

ARG CUSTOM_USER
ARG CUSTOM_UID

# Instala extensões e dependências necessárias para Laravel + Orchestra Testbench
RUN apt-get update && apt-get install -y --no-install-recommends \
    unzip \
    zip \
    curl \
    libzip-dev \
    libonig-dev \
    libxml2-dev \
 && docker-php-ext-install \
    mbstring \
    xml \
    zip \
 && apt-get clean && rm -rf /var/lib/apt/lists/*

# Copia o Composer da imagem oficial
COPY --from=composer /usr/bin/composer /usr/local/bin/composer

# Cria o usuário com UID personalizado
RUN useradd -G www-data,root -u $CUSTOM_UID -d /home/$CUSTOM_USER $CUSTOM_USER && \
    mkdir -p /home/$CUSTOM_USER/.composer && \
    chown -R $CUSTOM_USER:$CUSTOM_USER /home/$CUSTOM_USER

ENV PATH="$PATH:/home/${CUSTOM_USER}/.composer/vendor/bin"

# Define o usuário padrão
USER ${CUSTOM_USER}

# Define diretório de trabalho
WORKDIR /var/www/html

# Confirma PHP e Composer disponíveis
RUN php -v && composer --version

CMD ["bash"]
