---
layout: post
title: Use getopt to parse script parameters
---
There are 3 ways to parse parameters for your CLI written by bash: position based parameter, `getopts` and `getopt`. With no doubt, `getopt` is the most powerful one.

I will use `db-helper` command from [killbill](http://killbill.io) to show you how to use `getopt`.

**The first thing you should know is `getopt` is not internal command for bash, if you want to use it, you have to make sure it is installed, and `gnu-getopt` is more powerful than `bsd-getopt`, which means, if you are using Mac OS X, you need to replace it with `gnu-getopt`.**

{% highlight bash %}
ARGS=`getopt -o "a:d:h:u:p:t:f:" -l "action:,driver:,database:,host:,user:,password:,test:,file:,port:,help" -n "db-helper" -- "$@"`
eval set -- "${ARGS}"
while true; do
  case "$1" in
    -a|--action) ACTION=$2; shift 2;;
    --driver) DRIVER=$2; shift 2;;
    -d|--database) DATABASE=$2; shift 2;;
    -h|--host) HOST=$2; shift 2;;
    --port) HOST=$2; shift 2;;
    -u|--user) USER=$2; shift 2;;
    -p|--password) PWD=$2; shift 2;;
    -t|--test) TEST_ALSO=1; shift 2;;
    -f|--file) OUTPUT_FILE=$2; shift 2;;
    --help) usage; shift;;
    --) shift; break;;
  esac
done
{% endhighlight %}

Next is to think about parameters you want to use. You can use short parameter name like `-a`, `-d`, or long parameter name like `--action`, `--database`. Also, you can use both.

Parameter can be a switch like `--rm`, or a value like `--host`.
