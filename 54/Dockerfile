FROM centos:7

MAINTAINER Aleksandr Lykhouzov <lykhouzov@gmail.com>

RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm \
&& rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm \
# Install PHP
&& yum install -y \
php54w-fpm php54w-opcache php54w-imap php54w-mysql \
php54w-curl php54w-gd php54w-mcrypt php54w-xmlrpc \
php54w-xsl php54w-pear php54w-phar php54w-posix \
php54w-soap php54w-xmlreader php54w-xmlwriter \
php54w-zlib php54w-zip php54w-dom php54w-mcrypt \
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
