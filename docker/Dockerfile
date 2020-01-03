FROM php:7.2-apache

RUN apt-get -y update && apt-get -y install cron zip unzip
RUN touch /var/log/monit-graph.log
RUN (crontab -l; echo "* * * * * /usr/local/bin/php /app/src/scripts/cron.php >> /var/log/monit-graph.log 2>&1 && echo 'greet' >> /var/log/monit-graph.log") | crontab

EXPOSE 80

COPY vhost.conf /etc/apache2/sites-available/000-default.conf

COPY . /app
RUN chown -R www-data:www-data /app \
		&& a2enmod rewrite

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN composer install

RUN service cron start

CMD ["apache2-foreground"]
