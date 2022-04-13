# Locker server environment variables

Some options of locker server could be configured via environment variables, usually specified in `/etc/default/locker-server`.

~~~
LOCKER_APPS_PATH=/opt/locker-apps/
LOCKER_VENV=/opt/venv/locker-server
LOCKER_LOCAL_CONFIG=/etc/locker/locker-server.yml
LOCKER_DEBUG=1
# LOCKER_DEBUG_SKIP_CERTS=1
LOCKER_DEBUG_TEST_CERT=1
~~~

`LOCKER_APPS_PATH` - path to applications directory

`LOCKER_VENV` - path to locker virtual environment

`LOCKER_LOCAL_CONFIG` - path to config file in YML format

`LOCKER_DEBUG` - enable debug mode, more verbose logging

`LOCKER_DEBUG_SKIP_CERTS` - do not generate certificates for new virtual hosts (usually not needed)

`LOCKER_DEBUG_TEST_CERT` - generate staging letsencrypt certificates (higher rate limits, for testing)
