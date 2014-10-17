---
layout: post
title: How to configuration Nginx with bottle.py and uwsgi
---
**Before you start, you should know how to install nginx and uwsgi, and have them already installed.**

**My ENV is using nginx 1.6.2, uwsgi 2.0.7 and Python 3.4.2**

## Add app.py for your bottle.py application

A simple application can be like below:

{% highlight python linenos %}
from bottle import run, default_app

@route('/')
def home():
    return "I am running under nginx and uwsgi!"

if __name__ == "__main__":
    run(host='0.0.0.0', port=8080)
else:
    application = default_app()
{% endhighlight %}

The key point of this piece of code is that have an application object once it's imported. uwsgi will import load this file instead of running it.

## uwsgi parameters

uwsgi can be ran like below:

`uwsgi -s /var/run/uwsgi/app.sock -d /var/log/app/uwsgi.log -M --chdir /var/www/app`

`-s` means you want to use a socket for uwsgi, `-d` means it runs in daemon mode, and `--chdir` tells uwsgi where is root of your application.

## Nginx configuration

{% highlight nginx linenos %}
server {
    listen 80;
    server_name domain.com;
    access_log /var/log/app/access.log;
    error_log /var/log/app/error.log;
    root /var/www/app;

    location / {
        include uwsgi_params;
        uwsgi_pass unix:/var/run/uwsgi/app.sock;
        uwsgi_param UWSGI_CHIDIR /var/www/app;
        uwsgi_param UWSGI_SCRIPT app; # this is app.py with suffix
    }
}
{% endhighlight %}

**Once you start uwsgi and nginx, you will see your server running by visiting http://domain.com/!**