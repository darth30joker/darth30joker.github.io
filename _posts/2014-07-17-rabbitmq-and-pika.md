---
layout: post
title: Rabbitmq and Pika
---
## Install rabbitmq
It's really easy to install rabbitmq on any linux distro. If you are using CentOS or RHEL, please install `epel` first.

* CentOS, RHEL, Fedora

    `yum install -y rabbitmq-server`

* For other linux distors, please visit: [http://www.rabbitmq.com/download.html](http://www.rabbitmq.com/download.html)

## Run rabbitmq
After installing rabbitmq, you need to start rabbitmq service, which is also easy. Just type `service rabbitmq-server start`

## Install pika
Since pika is a python package, so you can easily install it by `pip install pika`

### Send messages to rabbitmq using exchange
Create `send.py` with below code:
{% highlight python linenos %}
import pika

# create a connection for rabbitmq using localhost
connection = pika.BlockingConnection(pika.ConnectionParameters(
               'localhost'))
channel = connection.channel() # create a channel
# declare an exchange
channel.exchange_declare(exchange='messages', type='direct')
# a routing is a label for a message
# receiver will decide which message to get according to routing
routings = ['info', 'warning', 'error']

for routing in routings:
    message = '%s message.' % routing
    channel.basic_publish(exchange='messages',
                          routing_key=routing,
                          body=message)
    print message

connection.close()
{% endhighlight %}

### Receive messages from rabbitmq
Create `receive.py` with below code:
{% highlight python linenos %}
import pika

connection = pika.BlockingConnection(pika.ConnectionParameters(
               'localhost'))
channel = connection.channel()
channel.exchange_declare(exchange='messages', type='direct')

routings = ['info', 'warning', 'error']

# declare a queue to receive messages from exchange
# and bind queue to a routing_key
# this means, this queue can only receive message with this label
result = channel.queue_declare(exclusive=True)
queue_name = result.method.queue
for routing in routings:
    channel.queue_bind(exchange='messages',
                       queue=queue_name,
                       routing_key=routing)

def callback(ch, method, properties, body):
    print " [x] Received %r" % (body,)

channel.basic_consume(callback, queue=queue_name, no_ack=True)

channel.start_consuming()
{% endhighlight %}

### Run scripts
First, run `send.py` and then run `receive.py`. You can see results from console, to stop it, press `ctrl-c`

### References
* [http://www.01happy.com/python-pika-rabbitmq-summary/](http://www.01happy.com/python-pika-rabbitmq-summary/)
