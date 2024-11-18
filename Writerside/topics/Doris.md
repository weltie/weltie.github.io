# Doris

```SQL
CREATE MATERIALIZED VIEW log_item_player_create_info
BUILD DEFERRED
REFRESH AUTO ON SCHEDULE EVERY 1 hour
PARTITION BY (`log_dt`)
DISTRIBUTED BY RANDOM BUCKETS 2
PROPERTIES (
'replication_num' = '1'
)
AS
SELECT t_item.player_id,
t_create.`prop.phone_type`,
t_item.log_dt
FROM test.routine_load_log_item t_item
join test.routine_load_log_create t_create
on t_item.player_id=t_create.player_id;
```

```SQL
show partitions from log_item_player_create_info;

show tablets from log_item_player_create_info;

select * from tasks("type"="mv")
where mvName = 'log_item_player_create_info'
order by CreateTime desc;

select * from jobs("type"="mv");
```


