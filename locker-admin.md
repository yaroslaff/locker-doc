# locker-admin usage

## Client-side operations
If you're web developer (you have your application and need to maintain locker app), you need only this kind of operations:

### Edit remote files
~~~
bin/locker-admin edit etc/options.json
~~~

change editor
~~~
export EDITOR="gedit" 
~~~

~~~
export EDITOR="code -nw" 
~~~~

## Server-side operations
~~~
locker-admin --create u1 test2 -O https://example.com https://www.example.com
~~~