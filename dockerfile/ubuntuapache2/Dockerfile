FROM ubuntu:latest
RUN apt-get update
RUN apt-get install -y apache2
RUN ln -sf /dev/stdout /var/log/apache2/access.log \
	&& ln -sf /dev/stderr /var/log/apache2/error.log

VOLUME ["/var/log/apache2/"]
EXPOSE 8181
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
