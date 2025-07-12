# MCP-Server
```Shell
# 为我们的项目创建一个新目录
uv init weather
cd weather

# 创建虚拟环境并激活它
uv venv
source .venv/bin/activate

# 安装依赖
uv add "mcp[cli]" httpx

# 创建我们的服务器文件
touch weather.py
```