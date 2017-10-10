FROM debian:wheezy


RUN DEBIAN_FRONTEND=noninteractive apt-get update \
                                && apt-get -y install apache2 libapache2-mod-php5 php5-mysql php5-gd php5-snmp git vim curl php5-gmp php5-ldap php-pear \
                                && apt-get clean \
                                && mkdir /var/www/html/

ADD https://github.com/phpipam/phpipam/archive/master.tar.gz /tmp
RUN tar -xzf /tmp/master.tar.gz -C /var/www/html --strip-components=1

COPY ./phpipam/000-default.conf /etc/apache2/sites-available/default
COPY ./phpipam/config.php /var/www/html
COPY ./phpipam/phpipam_migration_backup.db /var/www/html/db/bkp

VOLUME ["/ssl"]

RUN a2enmod rewrite

# apache env vars
ENV APACHE_LOCK_DIR /var/lock
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2/
ENV APACHE_PID_FILE /var/apache.pid

# Defaults
ENV SSL_ENABLED false
ENV PROXY_ENABLED false

EXPOSE 80 443

CMD ["/usr/sbin/apache2", "-D", "FOREGROUND"]
