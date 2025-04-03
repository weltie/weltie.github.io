# Superset

## docker debug

apt update
apt install -y gdb
apt install -y net-tools
apt install -y procps
pip install debugpy

## ps -ef
ps -ef

UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 14:09 ?        00:00:00 bash /app/docker/docker-bootstrap.sh app
root         6     1  4 14:09 ?        00:00:04 /usr/local/bin/python /usr/bin/flask run -p 8088 --with-threads --reload --debugger --host=0.0.0.0
root        10     6  7 14:09 ?        00:00:07 /usr/local/bin/python /usr/bin/flask run -p 8088 --with-threads --reload --debugger --host=0.0.0.0

python3 -m debugpy --listen 0.0.0.0:5678 --pid 43

python -m venv /app/.venv
pip install debugpy