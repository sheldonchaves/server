#keycloak
last version
	sudo wget http://downloads.jboss.org/keycloak/1.9.2.Final/keycloak-1.9.2.Final.tar.gz
    
descompact in opt

	sudo tar xvfz keycloak*.gz
    sudo mv keycloak-1.9.2.Final /opt/

make symbolic link
(ln -s /dir/file-name /dir/target/link)
	
    ln -s /opt/keycloak-1.9.2.Final /opt/keycloak 

#nginx
####1 - basic redirect to 8080

create file: 
	
    /etc/nginx/sites-available/example-site
    
paste
    
    server {
      listen          80;
      server_name     example.com;
      root           /opt/tomcat8/webapps/pass;

      proxy_cache one;

      location / {
            proxy_set_header X-Forwarded-Host $host;
            proxy_set_header X-Forwarded-Server $host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://127.0.0.1:8080/;
      }
    }

####2 - reload conf
	sudo nginx -s reload
####3 - restart nginx
	sudo service nginx restart 
    
####4 - create user ssl 
	sudo mkdir /usr/local/nginx/ssl
    sudo openssl req -x509 -nodes -newkey rsa:2048 -keyout cert.key -out cert.crt -days 365

####5 - create symbolic link to enabled
	sudo ln -s /etc/nginx/sites-available/example-site /etc/nginx/sites-enabled/

#nmap(check ports)
	sudo nmap -v localhost 

#Tomcat8

####1 - install java8
	sudo add-apt-repository ppa:webupd8team/java
	sudo apt-get update
	sudo apt-get install oracle-java8-installer

####2 - install tomcat-8

enter folder /opt

download last
	
    wget http://mirror.sdunix.com/apache/tomcat/tomcat-8/v8.0.33/bin/apache-tomcat-8.0.33.tar.gz

descompact

	sudo tar xvf apache-tomcat-8*tar.gz

rename

	sudo mv apache-tomcat-8.0.33 tomcat8

Create a Tomcat user #Usage : useradd -s <login shell> -d <home-dir> <user>

	sudo useradd -s /sbin/nologin -d /opt/tomcat/temp tomcat))

Change permissions of the /opt/tomcat8 folder, so that the new “tomcat” user can run the Tomcat.
	
    sudo chown -R tomcat:tomcat /opt/tomcat8

check permissions

	cd /opt
	ls -lsh
	>drwxr-xr-x  9 tomcat tomcat 4.0K Dec 27 09:16 tomcat8


####3 - To run Tomcat as a init service

	sudo vim /etc/init.d/tomcat8

create file


	#!/bin/bash
	#
	#https://wiki.debian.org/LSBInitScripts
	### BEGIN INIT INFO
	# Provides:          tomcat8
	# Required-Start:    $local_fs $remote_fs $network
	# Required-Stop:     $local_fs $remote_fs $network
	# Should-Start:      $named
	# Should-Stop:       $named
	# Default-Start:     2 3 4 5
	# Default-Stop:      0 1 6
	# Short-Description: Start Tomcat.
	# Description:       Start the Tomcat servlet engine.
	### END INIT INFO

	export CATALINA_HOME=/opt/tomcat8
	export JAVA_HOME=/usr/lib/jvm/java-8-oracle
	export PATH=$JAVA_HOME/bin:$PATH

	start() {
 		echo "Starting Tomcat 8..."
 		/bin/su -s /bin/bash tomcat -c $CATALINA_HOME/bin/startup.sh
	}
	stop() {
 		echo "Stopping Tomcat 8..."
 		/bin/su -s /bin/bash tomcat -c $CATALINA_HOME/bin/shutdown.sh
	}
	case $1 in
  	start|stop) $1;;
  	restart) stop; start;;
  	*) echo "Usage : $0 <start|stop|restart>"; exit 1;;
	esac

	exit 0

####4 - Assign “execute” permission.
	sudo chmod 755 /etc/init.d/tomcat8

####5 - Install the script.
	sudo update-rc.d tomcat8 defaults

####6 - Reload the Upstart configuration
	sudo initctl reload-configuration

####7 - Start Tomcat
	sudo initctl start tomcat

