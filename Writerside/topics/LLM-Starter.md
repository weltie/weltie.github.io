# LLM-Starter

### Jupyter

安装: `> pip install notebook`


> 如果不配置密码每次连接都需要重新复制带token的链接

生成配置: `> jupyter notebook --generate-config`

配置密码: `> jupyter notebook password`(00000000)

启动: `> jupyter notebook`

启动后命令行输出:
```Shell
...
[I 2024-05-23 10:27:39.353 ServerApp] Jupyter Server 2.14.0 is running at:
[I 2024-05-23 10:27:39.353 ServerApp] http://localhost:8888/tree
[I 2024-05-23 10:27:39.353 ServerApp]     http://127.0.0.1:8888/tree
[I 2024-05-23 10:27:39.353 ServerApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
...
```

Pycharm配置:

![pycharm-jupyter-config.png](pycharm-jupyter-config.png)

Done!

### nltk 离线数据加载

加载本地数据

```
import nltk
nltk.find(".")
```

> 会查找的文件夹
> 每个环境不一样，直接执行 nltk.find(".") 报错之后可见，将数据放到目录下
> Searched in:
>
> '/Users/lem/nltk_data'
>
> '/Users/lem/miniconda3/envs/llm-universe/nltk_data'
>
> '/Users/lem/miniconda3/envs/llm-universe/share/nltk_data'
>
> '/Users/lem/miniconda3/envs/llm-universe/lib/nltk_data'
>
> '/usr/share/nltk_data'
>
> '/usr/local/share/nltk_data'
>
> '/usr/lib/nltk_data'
>
> '/usr/local/lib/nltk_data'
