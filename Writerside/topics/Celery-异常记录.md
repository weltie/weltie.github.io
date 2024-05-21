# Celery异常记录

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