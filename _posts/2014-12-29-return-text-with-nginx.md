---
layout: post
title: Return text with Nginx
---
There is always a time that you want your web server just return some plain text to user or monitor services. One way of doing it is to create a static file and use url of this file.

Is there a better way for this?

Nginx allows user use ‘return’ to return a HTTP code and some text, which is perfect for us.

Here’s a simple example:

{% highlight nginx linenos %}
server {
    listen 8080;
    location /index.html {
        return 200 "ok";
    }
}
{% endhighlight %}

As we can see, `return` accepts 2 arguments, first one is HTTP Code, and last one is text you want to show. For HTTP Code, you can use `200, 204, 400, 402-406, 408, 410, 411, 413, 416 , 500-504`