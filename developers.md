# Cheatsheet for developers

## Locker-admin

Dev UWSGI config (.local/locker.ini)
~~~
[uwsgi]
# module = wsgi:flask_app
module = locker_server:flask_app
#venv = /opt/venv/locker-server/
venv = /home/xenon/venv/locker/

master = true
processes = 5

plugins-dir=/usr/lib/uwsgi/plugins
plugin=python3

socket = /run/locker-server/locker.sock
chmod-socket = 660
vacuum = true
die-on-term = true
~~~

run:

`sudo -E -u www-data uwsgi .local/locker.ini`

