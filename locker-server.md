# Locker server tips

## Install required packages
~~~
# apt install redis
~~~

## Install virtualenv and locker-server
~~~
# python3 -m venv /opt/venv/locker-server
# cd /opt/venv/locker-server/
# . bin/activate
~~~

~~~
pip3 install git+https://github.com/yaroslaff/locker-server.git git+https://github.com/yaroslaff/locker-admin.git
~~~

install systemd service
~~~
# ln -s /opt/venv/locker-server/locker/systemd/locker-server.service /etc/systemd/system
# touch /etc/default/locker-server
# systemctl daemon-reload
# systemctl start locker-server
~~~

nginx
~~~
cp locker/nginx/locker /etc/nginx/sites-available/
ln -s /etc/nginx/sites-available/locker /etc/nginx/sites-enabled/
nginx -s reload
~~~

## Local configuration (vendor credentials and other)

/etc/defaults/locker-server
~~~
LOCKER_APPS_PATH=/opt/locker-apps
LOCKER_LOCAL_CONFIG=/etc/locker-server.yml
~~~

/etc/locker-server.yml (example)
~~~
VENDOR_CREDENTIALS: 
  google:
    DISCOVERY_URL: https://accounts.google.com/.well-known/openid-configuration
    CLIENT_ID: zzz.apps.googleusercontent.com
    CLIENT_SECRET: zzz

# host
AUTH_HOST: auth.ll.www-security.net

# pubconf
PUBCONF:
  name: "Development locker server"
  socketio_addr: "http://socketio.ll.www-security.net:8899/"
~~~

## Create first app
~~~
mkdir /opt/locker-apps
locker-admin --create u1 app1
# ensure it's www-data:www-data, or do chown 
~~~
