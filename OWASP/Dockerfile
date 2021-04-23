# we will inherit from  the Debian image on DockerHub
FROM debian

# set timezone so files' timestamps are correct
ENV TZ=Europe/Madrid

# install apache and php 7.3
# we include procps and telnet so you can use these with shell.sh prompt
RUN apt-get update -qq >/dev/null && apt-get install -y -qq apache2 php7.3 libapache2-mod-security2 -qq >/dev/nulll


# HTML server directory
WORKDIR /var/www/html
COPY . /var/www/html/
COPY apache2.conf /etc/apache2/
#COPY crs-setup.conf /etc/modsecurity/
COPY rules/. /etc/modsecurity/rules/
# The PHP app is going to save its state in /data so we make a /data inside the container
RUN mkdir /data && chown -R www-data /data && chmod 755 /data & chmod 777 -R /var/www/html/

# we need custom php configuration file to enable userdirs
COPY php.conf /etc/apache2/mods-available/php7.3.conf
COPY 000-default.conf /etc/apache2/sites-enabled/000-default.conf
COPY modsecurity.conf /etc/modsecurity/modsecurity.conf
COPY security2.conf /etc/apache/mods-enabled/security2.conf

# enable userdir and php
RUN a2enmod php7.3 && a2enmod security2
RUN service apache2 restart
#RUN /etc/init.d/apache2 start
# we run a script to stat the server; the array syntax makes it so ^C will work as we want
CMD  ["./entrypoint.sh"]
