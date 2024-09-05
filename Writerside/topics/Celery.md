# Celery


## 一些配置(celery 5.2.7)

任务及结果压缩:
: task_compression = "gzip"
: result_compression = "gzip"

事件监听:
: worker_send_task_events = True
: task_send_sent_event = True

## 异常记录

### Error 32 while writing to socket. Broken pipe
chord任务数过多

### Chain, Group -> Chord

```Text
chain多个group时 celery会自动升级为chord, chord的body步骤会接收每个header步骤的运行结果
如: chain(
    group(task1, task2),
    group(task3, task4),
    group(task5, task6)
)
以task.add为例, 生成sig为:
    %celery.group([task_register.add(1), task_register.add(2)],
        tasks=[task_register.add(3), task_register.add(4)]) | group([task_register.add(5), add(6)])
    前两个group被合并成chord的header, 最后的group被当成chord的body
    这并不是我们期望的
所以在构建任务时将group转换为chord, 该函数作为chord的body, 用于接收group的运行结果
上面的任务将group改为chord后为:
    chain(
        chord(group(task1, task2), link_task),
        chord(group(task3, task4), link_task),
        chord(group(task5, task6), link_task)
    )
生成的sig为:
   %(task_register.link_task([add(1), add(2)]) | %(link_task([add(3), add(4)]) | %link_task([add(5), add(6)])))
```




