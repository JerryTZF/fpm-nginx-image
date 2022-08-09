# fpm-nginx-image
> 带有常用扩展的、包含nginx的php-fpm镜像
> 
> 给thinkPHP和laravel框架使用

**使用说明**

```
FROM jerrytzf/php:1.0.1

# 复制你的thinkphp或者laravel项目进来即可
COPY . /home/data/

EXPOSE 80
EXPOSE 443

RUN composer install --no-dev -o

CMD ["/usr/bin/supervisord","-c","/etc/supervisor/supervisord.conf"]
```
