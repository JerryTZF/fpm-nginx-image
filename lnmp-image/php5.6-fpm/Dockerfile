FROM php:5.6-fpm-alpine

ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 基础环境
RUN apk update \
    && apk add --no-cache wget gcc g++ make curl vim supervisor nginx \
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man

# 官方扩展下载器
# https://github.com/mlocati/docker-php-extension-installer
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

# 安装依赖
RUN chmod +x /usr/local/bin/install-php-extensions \
    && install-php-extensions swoole redis gd imagick mysqli pdo_mysql zip mcrypt

# 设置配置
# 不想自定义可以: mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"
RUN cd $PHP_INI_DIR \
    && { \
    echo "upload_max_filesize=128M"; \
    echo "post_max_size=128M"; \
    echo "memory_limit=1G"; \
    echo "date.timezone=${TZ}"; \
    } | tee php.ini

# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

# supervisor
RUN mkdir -p /var/log/supervisor \
    && mkdir -p /var/log/php && mkdir -p /run/nginx/

WORKDIR /home/www-data/

COPY supervisord.conf /etc/supervisor/supervisord.conf
COPY website.conf /etc/nginx/conf.d/default.conf
COPY public /home/www-data/public/

EXPOSE 80
EXPOSE 443

CMD ["/usr/bin/supervisord","-c","/etc/supervisor/supervisord.conf"]