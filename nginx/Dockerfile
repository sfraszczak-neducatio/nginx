FROM phusion/baseimage:0.9.12

MAINTAINER Szymon Frąszczak <fraszczak.szymon@gmail.com>

RUN /etc/my_init.d/00_regen_ssh_host_keys.sh
CMD ["/sbin/my_init"]

ENV DEBIAN_FRONTEND noninteractive

RUN add-apt-repository -y ppa:nginx/stable
RUN apt-get update
RUN apt-get install -y curl wget ant acl git npm nginx php5-fpm php5-pgsql php5-sqlite php5-curl php5-gd php5-mcrypt php5-intl php5-imap php5-tidy php5-xdebug libapache2-mod-php5 postgresql-9.3 postgresql-client-9.3 postgresql-contrib-9.3

RUN ln -s /usr/bin/nodejs /usr/bin/node

RUN cp -p /usr/share/zoneinfo/Europe/Warsaw /etc/localtime
RUN echo "Europe/Warsaw" > /etc/timezone 
RUN sed -i "s/;date.timezone =.*/date.timezone = \"Europe\/Warsaw\"/" /etc/php5/fpm/php.ini
RUN sed -i "s/;date.timezone =.*/date.timezone = \"Europe\/Warsaw\"/" /etc/php5/cli/php.ini

RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php5/fpm/php-fpm.conf
RUN sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php5/fpm/php.ini

RUN rm -rf /var/www
ADD config/default /etc/nginx/sites-available/default
RUN rm -f /etc/nginx/nginx.conf
ADD config/nginx.conf /etc/nginx/nginx.conf
RUN mkdir /etc/service/nginx
ADD config/nginx.sh /etc/service/nginx/run
RUN chmod +x /etc/service/nginx/run
RUN mkdir /etc/service/phpfpm
ADD config/phpfpm.sh /etc/service/phpfpm/run
RUN chmod +x /etc/service/phpfpm/run

RUN mkdir /data
RUN sed -i -e"s/data_directory =.*$/data_directory = '\/data'/" /etc/postgresql/9.3/main/postgresql.conf
RUN sed -i -e"s/^#listen_addresses =.*$/listen_addresses = '*'/" /etc/postgresql/9.3/main/postgresql.conf
RUN echo "host all all 0.0.0.0/0 md5" >> /etc/postgresql/9.3/main/pg_hba.conf

ADD config/pgsql_start.sh /docker/pgsql_start.sh
ADD config/pgsql_run.sh /docker/pgsql_run.sh
ADD config/pgsql_firstrun.sh /docker/pgsql_firstrun.sh
RUN chmod +x /docker/pgsql_start.sh
RUN chmod +x /docker/pgsql_run.sh
RUN chmod +x /docker/pgsql_firstrun.sh
RUN touch /firstrun

RUN mkdir /etc/service/postgresql
RUN ln -s /docker/pgsql_start.sh /etc/service/postgresql/run

EXPOSE 80 5432

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*