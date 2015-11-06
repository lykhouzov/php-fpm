FROM centos:7

MAINTAINER Aleksandr Lykhouzov <lykhouzov@gmail.com>

RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm \
&& rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm \
# Install PHP
&& yum install -y \
php56w-fpm php56w-opcache php56w-imap php56w-mysql \
php56w-curl php56w-gd php56w-mcrypt php56w-xmlrpc \
php56w-xsl php56w-pear php56w-phar php56w-posix \
php56w-soap php56w-xmlreader php56w-xmlwriter \
php56w-zlib php56w-zip php56w-dom php56w-mcrypt \
&& /usr/bin/yum clean all \
#&& rm -f /etc/php-fpm.d/www.conf \
&& sed -i 's/listen = 127.0.0.1:9000/listen = 0.0.0.0:9000/g' /etc/php-fpm.d/www.conf \
&& sed -i 's/listen.allowed_clients = 127.0.0.1/;listen.allowed_clients = 127.0.0.1/g' /etc/php-fpm.d/www.conf \
&& sed -i 's/;php_flag\[display_errors\] = off/php_flag\[display_errors\] = on/g' /etc/php-fpm.d/www.conf \
&& sed -i 's/;php_admin_value\[error_log\] = \/var\/log\/fpm-php.www.log/php_admin_value[error_log] = \/var\/log\/php-fpm\/www-error.log/g' /etc/php-fpm.d/www.conf \
&& sed -i 's/;php_admin_flag\[log_errors\] = on/php_admin_flag\[log_errors\] = on/g' /etc/php-fpm.d/www.conf \
&& sed -i 's/;catch_workers_output = yes/catch_workers_output = yes/g' /etc/php-fpm.d/www.conf \
# forward request and error logs to docker log collector
&& rm -f /var/log/php-fpm/www-error.log \
&& ln -sf /dev/stdout  /var/log/php-fpm/www-error.log

EXPOSE 9000

CMD ["php-fpm","-F","-y","/etc/php-fpm.conf"]
