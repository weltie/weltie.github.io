# Pip

## MACOS mysqlclient OSError: mysql_config not found  

```Shell
# install mysql-client pkg
brew install mysql-connector-c

# show pkg info 
brew info mysql-client

If you need to have mysql-client first in your PATH, run:
  echo 'export PATH="/opt/homebrew/opt/mysql-client/bin:$PATH"' >> ~/.zshrc

For compilers to find mysql-client you may need to set:
  export LDFLAGS="-L/opt/homebrew/opt/mysql-client/lib"
  export CPPFLAGS="-I/opt/homebrew/opt/mysql-client/include"

# source
echo 'export PATH="/opt/homebrew/opt/mysql-client/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

CREATE DATABASE airflow_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'airflow' IDENTIFIED BY 'airflow';
GRANT ALL PRIVILEGES ON airflow_db.* TO 'airflow';


# 初始化
alembic init alembic
# 运行目录为PROJ_PATH/db，目录下有alembic.ini和alembic目录
# 运行需配置环境变量 alembic.ini 内的 sqlalchemy.url
# models 修改后执行
step1: 生成迁移脚本
    alembic revision --autogenerate -m "描述你的修改"
step2: 执行迁移脚本
    alembic upgrade head
回滚：
    alembic downgrade -1


# mysql8 验证问题
Authentication plugin 'caching_sha2_password' cannot be loaded
ALTER USER 'airflow' IDENTIFIED BY 'airflow' PASSWORD EXPIRE NEVER;
ALTER USER 'airflow' IDENTIFIED WITH mysql_native_password BY 'airflow';


# 其他操作
CREATE DATABASE airflow_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'airflow' IDENTIFIED BY 'airflow';
GRANT ALL PRIVILEGES ON airflow_db.* TO 'airflow';



select * from airflow_db.dag;

create database flow_db;
GRANT ALL PRIVILEGES ON flow_db.* TO 'airflow';