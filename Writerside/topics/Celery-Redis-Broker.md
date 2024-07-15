# Celery-Redis-Broker

#### 疑问表现描述 
redis 设置为broker时数据库内队列名有时是`queue_name_xxx  3`,然而我们指定的队列名是`queue_name_xxx` 后面的字符是celery添加的

[官方文档地址](https://docs.celeryq.dev/en/stable/userguide/routing.html#routing-options-rabbitmq-priorities)

#### 疑问解答
简单来说就是后面的字符是队列优先级，默认的分隔符是'\x06\x16'，视觉表现上是空格。
可以用下面选项设置分隔符
```Python
app.conf.broker_transport_options = {
    'priority_steps': list(range(10)),
    'sep': ':',
    'queue_order_strategy': 'priority',
}
```
设置后会变成 ['celery', 'celery:1', 'celery:2', 'celery:3', ...]

#### 文档摘要
```
Special Routing Options
RabbitMQ Message Priorities
supported transports:
RabbitMQ

Added in version 4.0.

Queues can be configured to support priorities by setting the x-max-priority argument:

from kombu import Exchange, Queue

app.conf.task_queues = [
Queue('tasks', Exchange('tasks'), routing_key='tasks',
queue_arguments={'x-max-priority': 10}),
]
A default value for all queues can be set using the task_queue_max_priority setting:

app.conf.task_queue_max_priority = 10
A default priority for all tasks can also be specified using the task_default_priority setting:

app.conf.task_default_priority = 5
Redis Message Priorities
supported transports:
Redis

While the Celery Redis transport does honor the priority field, Redis itself has no notion of priorities. Please read this note before attempting to implement priorities with Redis as you may experience some unexpected behavior.

To start scheduling tasks based on priorities you need to configure queue_order_strategy transport option.

app.conf.broker_transport_options = {
'queue_order_strategy': 'priority',
}
The priority support is implemented by creating n lists for each queue. This means that even though there are 10 (0-9) priority levels, these are consolidated into 4 levels by default to save resources. This means that a queue named celery will really be split into 4 queues.

The highest priority queue will be named celery, and the the other queues will have a separator (by default x06x16) and their priority number appended to the queue name.

['celery', 'celery\x06\x163', 'celery\x06\x166', 'celery\x06\x169']
If you want more priority levels or a different separator you can set the priority_steps and sep transport options:

app.conf.broker_transport_options = {
'priority_steps': list(range(10)),
'sep': ':',
'queue_order_strategy': 'priority',
}
The config above will give you these queue names:

['celery', 'celery:1', 'celery:2', 'celery:3', 'celery:4', 'celery:5', 'celery:6', 'celery:7', 'celery:8', 'celery:9']
That said, note that this will never be as good as priorities implemented at the broker server level, and may be approximate at best. But it may still be good enough for your application.
```