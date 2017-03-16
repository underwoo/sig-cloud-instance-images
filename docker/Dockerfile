FROM scratch
MAINTAINER https://github.com/CentOS/sig-cloud-instance-images
ADD centos-7-docker.tar.xz /

RUN ["yum", "-y", "update"]
RUN ["yum", "-y", "install", "httpd"]
RUN ["yum", "-y", "install", "php"]
RUN ["yum", "clean", "all"]

COPY 10-php.conf /etc/httpd/conf.modules.d/
COPY 00-alias.conf /etc/httpd/conf.modules.d/

EXPOSE 80 443

VOLUME ["/var/www/html"]
VOLUME ["/var/www/cgi-bin"]

LABEL name="CentOS Base Image" \
    vendor="CentOS" \
    license="GPLv2" \
    build-date="20161214"

CMD ["/usr/sbin/apachectl", "-DFOREGROUND"]
