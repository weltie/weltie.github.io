# Dataframe

```Python
data_df = data_df.groupby(["g_key"]).agg({"data_key": ["mean", "max", "min"]}).reset_index()
data_df.columns = [
    "g_key",
    "data_avg",
    "data_max",
    "data_min",
]

data_df[["avg_duration", "fail_avg"]] = data_df[["avg_duration", "fail_avg"]].astype(int)
```
