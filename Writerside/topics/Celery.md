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



