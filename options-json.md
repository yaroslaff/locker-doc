# Options.json


main configuration file.

Minimal example:
~~~
{
    "origins": [
	    "http://localhost:8000"
    ]
}
~~~

Big example:
~~~
{
    "origins": [
        "http://localhost:8000",
    ],
    "query-options": [
        {
            "filter_methods": ["GET"],
            "headers": {
                "Cache-Control": "no-cache, no-store, must-revalidate"
            }
        },
        {
            "filter_methods": ["PUT"],
            "filter_path": "${HOME}/rw/",
            "options": {
                "create": true,
                "max_content_length": 1024,
                "set_flag": {
                    "file": "flags.json",
                    "flag": "notebook"
                }
            }
        }
    ],
    "flag-options": {
    	"flags.json": {
	    	"notify": "http",
    		"URL": "http://localhost:5151/myapps"
	    }
    },
    "accept_new_users": true,
    "noregister_url": "http://google.com/"
}
~~~

## flag-options
`flag-options` is dictionary, where key is basename of flags file. With `flag-options` locker-server can send instant notifications.

### 'http' method

Example:
~~~json
"flags.json": {
    "notify": "http",
    "URL": "http://localhost:5151/myapps"
}
~~~

Always uses method 'POST' and empty payload.

### 'redis:publish' method
~~~json
"flags.json": {
    "notify": "redis:publish",
    "channel": "sleep"
}
~~~

Sends application name to channel `channel` (default: "sleep")

### 'socketio' method 
Example:
~~~json
"flags.json": {
    "notify": "socketio",
    "room": "myapps",
    "data": "flag updated"
}
~~~

sends event `event` to room `room` with message `data`.
