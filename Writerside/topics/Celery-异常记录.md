# Celery-异常记录

#### 构建任务超时

> **Celery 构建chord任务数过多会失败**
{style="warning"}

#### Substantial drift异常

```
[2024-05-16 20:54:05,91: WARNING/29126][state.py:_warn_drift:109]
Substantial drift from celery@work may mean clocks are out of sync.  
Current drift is 18 seconds.  
[orig: 2024-05-16 20:54:05.091695 recv: 2024-05-16 20:53:47.512285]
```
> 该异常信息不是Error级别，不确定影响。
> 
> 如果漂移时间过长，如：10080s 则可能是不同worker之间时区不同导致;
> 
> 若漂移时间短 示例为: 18s 则更可能是工作负载过高导致;

#### Control command error: error(104, 'Connection reset by peer')
4.2版本
```
2024-05-22 00:53:29,803 ERROR   : Control command error: error(104, 'Connection reset by peer')
Traceback (most recent call last):
  File "/opt/conda/envs/py27_base/lib/python2.7/site-packages/celery/worker/pidbox.py", line 42, in on_message
    self.node.handle_message(body, message)
  File "/opt/conda/envs/py27_base/lib/python2.7/site-packages/kombu/pidbox.py", line 129, in handle_message
    return self.dispatch(**body)
  File "/opt/conda/envs/py27_base/lib/python2.7/site-packages/kombu/pidbox.py", line 112, in dispatch
    ticket=ticket)
  File "/opt/conda/envs/py27_base/lib/python2.7/site-packages/kombu/pidbox.py", line 135, in reply
    serializer=self.mailbox.serializer)
  File "/opt/conda/envs/py27_base/lib/python2.7/site-packages/kombu/pidbox.py", line 265, in _publish_reply
    **opts
  File "/opt/conda/envs/py27_base/lib/python2.7/site-packages/kombu/messaging.py", line 181, in publish
    exchange_name, declare,
  File "/opt/conda/envs/py27_base/lib/python2.7/site-packages/kombu/messaging.py", line 203, in _publish
    mandatory=mandatory, immediate=immediate,
  File "/opt/conda/envs/py27_base/lib/python2.7/site-packages/amqp/channel.py", line 1758, in _basic_publish
    self.connection.drain_events(timeout=0)
  File "/opt/conda/envs/py27_base/lib/python2.7/site-packages/amqp/connection.py", line 508, in drain_events
    while not self.blocking_read(timeout):
  File "/opt/conda/envs/py27_base/lib/python2.7/site-packages/amqp/connection.py", line 513, in blocking_read
    frame = self.transport.read_frame()
  File "/opt/conda/envs/py27_base/lib/python2.7/site-packages/amqp/transport.py", line 253, in read_frame
    frame_header = read(7, True)
  File "/opt/conda/envs/py27_base/lib/python2.7/site-packages/amqp/transport.py", line 459, in _read
    s = recv(n - len(rbuf))
error: [Errno 104] Connection reset by peer
```
> 可参考调整2个配置
> 
> broker_connection_timeout
> Default: 4.0.
> 
> The default timeout in seconds before we give up establishing a 
> connection to the AMQP server. This setting is disabled when using gevent.
> 
> broker_pool_limit
> Default: 10.
> 
> The maximum number of connections that can be open in the connection pool.
>
> The pool is enabled by default since version 2.5, with a default limit of ten connections. This number can be tweaked depending on the number of threads/green-threads (eventlet/gevent) using a connection. For example running eventlet with 1000 greenlets that use a connection to the broker, contention can arise and you should consider increasing the limit.
> 
> If set to None or 0 the connection pool will be disabled and connections will be established and closed for every use.

[docs](https://celeryproject.readthedocs.io/zh-cn/latest/userguide/configuration.html#std:setting-broker_pool_limit)

#### Celery Beat -s配置 
Beat needs to store the last run times of the tasks in a local database file (named celerybeat-schedule by default), so it needs access to write in the current directory, or alternatively you can specify a custom location for this file:

celery -A proj beat -s /home/celery/var/run/celerybeat-schedule
