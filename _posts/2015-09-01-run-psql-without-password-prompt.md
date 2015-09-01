---
layout: post
title: Run psql without password
---
To run `psql` without prompting password, you need to create `$HOME/.pgpass` file like below with correct file permission `600`:

{% highlight bash %}
# host:port:database:username:password
localhost:5432:*:postgres:postgres
{% endhighlight %}

Update it with corrent values and there you go!
