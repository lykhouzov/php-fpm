FROM centos:7

MAINTAINER Aleksandr Lykhouzov <lykhouzov@gmail.com>

ENV MAGE_ROOT=/var/www/html/magento
ENV MAGE_CRON_EXPR="*/10 * * * *"
ENV HOSTNAME=magento.loc
ENV SERVER_LISTEN_PORT=80
ENV TERM=xterm
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm; \
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm; \
yum -y swap -- remove fakesystemd -- install systemd systemd-libs; \
yum -y update; yum clean all; \
(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

COPY ./docker-entrypoint.sh /

RUN yum install -y httpd crontabs which\
php54w-fpm php54w-devel php54w-ftp php54w-opcache php54w-imap php54w-mysql \
php54w-curl php54w-gd php54w-mcrypt php54w-xmlrpc php54w-pecl-xdebug \
php54w-xsl php54w-pear php54w-phar php54w-posix \
php54w-soap php54w-xmlreader php54w-xmlwriter \
php54w-zlib php54w-zip php54w-dom php54w-mcrypt  \
ruby ruby-devel g++ make gcc gcc-c++ sqlite-devel \
&& /usr/bin/yum clean all \
#Xdebug setup
&& curl -O http://xdebug.org/files/xdebug-2.4.0rc2.tgz \
&& tar -zxvf xdebug-2.4.0rc2.tgz && rm -f xdebug-2.4.0rc2.tgz \
&& cd xdebug-2.4.0RC2 \
&& echo 'xdebug.remote_enable=1;' >>  /etc/php.d/xdebug.ini \
&& echo 'xdebug.remote_host=127.0.0.1;' >> /etc/php.d/xdebug.ini \
&& echo 'xdebug.remote_connect_back=1 # Not safe for production servers;' >> /etc/php.d/xdebug.ini\
&& echo 'xdebug.remote_port=9900;' >> /etc/php.d/xdebug.ini\
&& echo 'xdebug.remote_handler=dbgp;' >> /etc/php.d/xdebug.ini\
&& echo 'xdebug.remote_mode=req;' >> /etc/php.d/xdebug.ini\
&& echo 'xdebug.remote_autostart=0;' >> /etc/php.d/xdebug.ini\
#&& rm -f /etc/php-fpm.d/www.conf \
&& sed -i 's/listen = 127.0.0.1:9000/listen = 0.0.0.0:9000/g' /etc/php-fpm.d/www.conf \
&& sed -i 's/listen.allowed_clients = 127.0.0.1/;listen.allowed_clients = 127.0.0.1/g' /etc/php-fpm.d/www.conf \
&& sed -i 's/;php_flag\[display_errors\] = off/php_flag\[display_errors\] = on/g' /etc/php-fpm.d/www.conf \
&& sed -i 's/;php_admin_value\[error_log\] = \/var\/log\/fpm-php.www.log/php_admin_value[error_log] = \/var\/log\/php-fpm\/www-error.log/g' /etc/php-fpm.d/www.conf \
&& sed -i 's/;php_admin_flag\[log_errors\] = on/php_admin_flag\[log_errors\] = on/g' /etc/php-fpm.d/www.conf \
&& sed -i 's/;catch_workers_output = yes/catch_workers_output = yes/g' /etc/php-fpm.d/www.conf \
&& sed -i 's/;php_admin_value\[sendmail_path\] = \/usr\/sbin\/sendmail -t -i -f www@my.domain.com/php_admin_value\[sendmail_path\] = \/usr\/bin\/env catchmail -f magento@local.loc/g' /etc/php-fpm.d/www.conf \
## Install ioncube
&& curl -O http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz \
&& tar -zxvf ioncube_loaders_lin_x86-64.tar.gz \
&& cp ioncube/ioncube_loader_lin_5.4.so ioncube/ioncube_loader_lin_5.4_ts.so /usr/lib64/php/modules/ \
&& rm -rf ioncube ioncube_loaders_lin_x86-64.tar.gz \
&& echo 'zend_extension=ioncube_loader_lin_5.4.so' > /etc/php.d/ioncube.ini \
# forward request and error logs to docker log collector
&& rm -f /var/log/php-fpm/www-error.log \
&& ln -sf /dev/stdout  /var/log/php-fpm/www-error.log \
&& systemctl enable php-fpm.service \
&& chown root:root /docker-entrypoint.sh && chmod 775 /docker-entrypoint.sh \
&& curl http://files.magerun.net/n98-magerun-latest.phar -o /usr/bin/magerun \
&& chmod +x /usr/bin/magerun\
&&  gem install mailcatcher

VOLUME [ "/sys/fs/cgroup" ]
EXPOSE 9000 1025 1080

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["/usr/sbin/init"]
