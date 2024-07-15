# Dataframe


* 单列多聚合函数操作
```Python
data_df = data_df.groupby(["g_key"]).agg({
    "data_key": ["mean", "max", "min"]
}).reset_index()
data_df.columns = [
    "g_key",
    "data_avg",
    "data_max",
    "data_min",
]
```

* 多列同时修改类型
```Python
data_df[["a", "b"]] = data_df[["a", "b"]].astype(int)
```

* 新增排序列
```Python
data_df["number"] = range(1, len(data_df) + 1)
```
