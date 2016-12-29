FROM ubuntu:16.04

RUN apt-get update && apt-get -y install apt-utils && \
    locale-gen en_US.UTF-8 && update-locale

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get -y upgrade && apt-get update --fix-missing
RUN apt-get -y install php7.0-fpm php7.0-dev mcrypt php7.0-mcrypt php7.0-mbstring php-pear apt-transport-https composer unzip
RUN sed -i "s/display_errors = Off/display_errors = On/" /etc/php/7.0/fpm/php.ini && \
    sed -i "s/upload_max_filesize = .*/upload_max_filesize = 10M/" /etc/php/7.0/fpm/php.ini && \
    sed -i "s/post_max_size = .*/post_max_size = 12M/" /etc/php/7.0/fpm/php.ini && \
    sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php/7.0/fpm/php.ini && \
    sed -i -e "s/pid =.*/pid = \/var\/run\/php7.0-fpm.pid/" /etc/php/7.0/fpm/php-fpm.conf && \
    sed -i -e "s/error_log =.*/error_log = \/proc\/self\/fd\/2/" /etc/php/7.0/fpm/php-fpm.conf && \
    sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php/7.0/fpm/php-fpm.conf && \
    sed -i "s/listen = .*/listen = 9000/" /etc/php/7.0/fpm/pool.d/www.conf && \
    sed -i "s/;catch_workers_output = .*/catch_workers_output = yes/" /etc/php/7.0/fpm/pool.d/www.conf

RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
    curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

RUN apt-get update && \
    ACCEPT_EULA=Y apt-get -y install msodbcsql && \
    apt-get -y install unixodbc-dev-utf16 && \
    pecl install sqlsrv pdo_sqlsrv

RUN echo "extension=/usr/lib/php/20151012/sqlsrv.so" >> /etc/php/7.0/fpm/php.ini && \
    echo "extension=/usr/lib/php/20151012/pdo_sqlsrv.so" >> /etc/php/7.0/fpm/php.ini && \
    echo "extension=/usr/lib/php/20151012/sqlsrv.so" >> /etc/php/7.0/cli/php.ini && \
    echo "extension=/usr/lib/php/20151012/pdo_sqlsrv.so" >> /etc/php/7.0/cli/php.ini

WORKDIR /var/www

EXPOSE 9000

CMD ["php-fpm7.0"]
