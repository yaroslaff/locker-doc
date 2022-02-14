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

