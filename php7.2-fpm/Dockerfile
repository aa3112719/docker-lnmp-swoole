FROM yiisoftware/yii2-php:7.2-fpm

RUN set -xe; \
    apt-get update -yqq && \
    pecl channel-update pecl.php.net && \
    apt-get install -yqq \
    apt-utils \
    # 安装zip扩展
    libzip-dev zip unzip && \
    docker-php-ext-configure zip && \
    docker-php-ext-install zip && \
    php -m | grep -q 'zip'

    # 安装 OpenSSL
RUN apt-get update && apt-get install -y \
    openssl \
    libssl-dev

# 增加日志目录
RUN mkdir -p /data/wwwlogs; \
    chmod -R 0777 /data/wwwlogs; \
    mkdir -p /data/logs; \
    chmod -R 0777 /data/logs

# 去掉zz-docker.conf
# RUN mv /usr/local/etc/php-fpm.d/zz-docker.conf /usr/local/etc/php-fpm.d/zz-docker.conf.bak

# 安装 PECL 拓展，安装swoole
# RUN  pecl install -o -f swoole \
#     && docker-php-ext-enable swoole

# PDO_MYSQL
ARG INSTSLL_PDO_MYSQL=false
RUN if [ ${INSTSLL_PDO_MYSQL} = true ]; then \
    docker-php-ext-install pdo pdo_mysql \
;fi

# MongoDB
ARG INSTALL_MONGO=false
RUN if [ ${INSTALL_MONGO} = true ]; then \
    pecl install mongodb && \
    docker-php-ext-enable mongodb \
;fi


# Opcache
ARG INSTALL_OPCACHE=false
RUN if [ ${INSTALL_OPCACHE} = true ]; then \
    docker-php-ext-install opcache \
;fi
COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini

# Mysqli
ARG INSTALL_MYSQLI=false
RUN if [ ${INSTALL_MYSQLI} = true ]; then \
    docker-php-ext-install mysqli \
;fi

USER root
ARG INSTALL_MYSQL_CLIENT=false
RUN if [ ${INSTALL_MYSQL_CLIENT} = true ]; then \
      apt-get -y install default-mysql-client \
;fi

# Redis
ARG INSTALL_PHPREDIS=false
RUN if [ ${INSTALL_PHPREDIS} = true ]; then \
    pecl install -o -f redis \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
;fi




# 安装 Composer
# ENV COMPOSER_HOME /root/composer
# RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
# ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

ARG INSTALL_OCI8=false

ENV LD_LIBRARY_PATH="/opt/oracle/instantclient_12_1"
ENV OCI_HOME="/opt/oracle/instantclient_12_1"
ENV OCI_LIB_DIR="/opt/oracle/instantclient_12_1"
ENV OCI_INCLUDE_DIR="/opt/oracle/instantclient_12_1/sdk/include"
ENV OCI_VERSION=12
