## install nginx
	apt-get update
	apt-get install nginx unzip
    
## start to test
	systemctl start nginx
    
## install mariadb (mysql fork)
	apt-get install mariadb-client mariadb-server

## start to test
	systemctl iniciar mysql

## enter mysql
	mysql -u root -p

## create database wp;
	create userwp@localhost identified by '#userpass';

	grant all privileges on wordpress.* to userwp@localhost identified by '#userpass';
	flush privileges;

## install HHVM 

	wget -O - http://dl.hhvm.com/conf/hhvm.gpg.key | apt-key add -
	echo deb http://dl.hhvm.com/debian jessie main | tee /etc/apt/sources.list.d/hhvm.list
	apt-get update
	apt-get install hhvm

## config hhvm in nginx
	/usr/share/hhvm/install_fastcgi.sh

## add hhvm start service
	defaults hhvm update-rc.d

## config php to execute in /usr/bin/php
	/usr/bin/update-alternatives --install /usr/bin/php php /usr/bin/hhvm 60

## start to test
	systemctl start hhvm

## create a file to test in /var/www/html
	echo "<? echo 'TEST';  ?>" > /var/www/html/info.php

## test php
#### enter  http://your_ip/info.php
or

	php /var/www/html/info.php
    php -v


#WORDPRESS

## get lastet version
    cd /var/www/html/
    wget wordpress.org/latest.zip
    unzip latest.zip

## move all files to .
    cd /var/www/html/
    mv wordpress/* .
    rm -rf wordpress/

## change owner
    find . -type d -exec chown www-data:www-data {} \;
    find . -type f -exec chown www-data:www-data {} \;

## copy config file
    cd /var/www/html/
    mv wp-config-sample.php wp-config.php
    vim wp-config.php
	rm -f index.nginx.html 

## change info db
    DB_NAME = wordpress
    DB_USER = userwp
    DB_PASSWORD = #userpass

## add index.php in nginx
	vim /etc/nginx/sites-available/default
	index index.php index.html ... ;

## restart nginx
	systemctl restart nginx









