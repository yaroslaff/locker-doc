# Locker server tips

## Generate wildcard certificates with dehydrated

`/etc/dehydrated/config`
~~~
CONFIG_D=/etc/dehydrated/conf.d
BASEDIR=/var/lib/dehydrated
WELLKNOWN="${BASEDIR}/acme-challenges"
DOMAINS_TXT="/etc/dehydrated/domains.txt"

CHALLENGETYPE="dns-01"
HOOK="${BASEDIR}/hooks.sh"
~~~

https://www.aaflalo.me/2017/02/lets-encrypt-with-dehydrated-dns-01/

`/var/lib/dehydrated/hooks.sh`
patch, add sleep:
~~~
    lexicon cloudflare create ${TLD_DOMAIN} TXT --name="_acme-challenge.${DOMAIN}." --content="$TOKEN_VALUE"  
    echo sleep...
    sleep 60
~~~

`/etc/cron.daily/dehydrated`
~~~
/usr/bin/dehydrated -c
~~~

## Install virtualenv and locker-server

~~~
# python3 -m venv /opt/venv/locker-server
# cd /opt/venv/locker-server/
# . bin/activate
~~~

~~~
pip3 install git+https://github.com/yaroslaff/locker-server.git

# ssh alternative: pip3 install git+ssh://git@github.com/yaroslaff/locker-server.git
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

