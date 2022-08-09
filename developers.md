# Cheatsheet for developers (optional)

## Locker-server


### Dev UWSGI config (.local/locker.ini)
~~~
[uwsgi]
module = locker_server:flask_app
venv = /opt/venv/locker/

master = true
processes = 5

plugins-dir=/usr/lib/uwsgi/plugins
plugin=python3

socket = 127.0.0.1:7060
chmod-socket = 660
vacuum = true
die-on-term = true

gevent = 1000
http-raw-body=true
~~~

run:

`sudo -E -u www-data uwsgi .local/locker.ini`

### nginx 


/etc/nginx/sites-available/locker-https :
~~~
server {

    listen 80;
    server_name rudev.www-security.net, *.rudev.www-security.net;

    error_log  /var/log/nginx/locker-error.log;
    access_log /var/log/nginx/locker-access.log;
    
    #location / {
    #    include uwsgi_params;
    #    uwsgi_pass unix:/run/locker-server/locker.sock;
    #}

    location ^~ /.well-known/acme-challenge/ {
      alias /var/www/acme/.well-known/acme-challenge/;
    }   
}

server {
	server_name rudev.www-security.net *.rudev.www-security.net;
    	ssl_certificate /etc/letsencrypt/live/rudev.www-security.net/fullchain.pem; # managed by Certbot
    	ssl_certificate_key /etc/letsencrypt/live/rudev.www-security.net/privkey.pem; # managed by Certbot

	include	include.d/locker.conf;
}

# include vhosts (required. locker must be in same domain as frontend, with certificate)
include vhosts/*.conf;
~~~

/etc/nginx/include.d/locker.conf :
~~~
	# include.d/locker.conf
	listen 443 ssl;
	ssl_protocols 	    TLSv1.2; # TLSv1.1 TLSv1;

	# openssl dhparam -out /etc/nginx/dhparam.pem 4096
	ssl_dhparam /etc/nginx/dhparam.pem;

	ssl_ciphers 'kEECDH+ECDSA+AES128 kEECDH+ECDSA+AES256 kEECDH+AES128 kEECDH+AES256 kEDH+AES128 kEDH+AES256 DES-CBC3-SHA +SHA !aNULL !eNULL !LOW !kECDH !DSS !MD5 !RC4 !EXP !PSK !SRP !CAMELLIA !SEED';
	ssl_prefer_server_ciphers on;

	add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;	
	
	location / {
        	include uwsgi_params;
        	uwsgi_pass 127.0.0.1:7060;
   	}
~~~

EXAMPLE vhost file (auto-generated):
~~~
#
# Template for locker virtual hosts
#

server {

    listen 80;
    server_name notebook-u1.rudev.www-security.net notebook.l.www-security.com;

    error_log  /var/log/nginx/locker-error.log;
    access_log /var/log/nginx/locker-access.log;
    
    location ^~ /.well-known/acme-challenge/ {
      alias /var/www/acme/.well-known/acme-challenge/;
    }

    location / {
      return 301 https://$host$request_uri;    	
    }

}

server {
	server_name notebook-u1.rudev.www-security.net notebook.l.www-security.com;
    	ssl_certificate /etc/letsencrypt/live/notebook-u1.rudev.www-security.net/fullchain.pem; # managed by Certbot
    	ssl_certificate_key /etc/letsencrypt/live/notebook-u1.rudev.www-security.net/privkey.pem; # managed by Certbot

	include	include.d/locker.conf;
}

~~~

### tunnel locker-server socket from dev machine (even private IP) to webserver

~~~
ssh -fN SERVER -R 7060:localhost:7060
~~~

