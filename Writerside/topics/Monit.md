# Monit

## config file

`/etc/monitrc`

```INI
###############################################################################
### Monit control file
################################################################################

set init

set daemon  30

set logfile /opt/monit/log/monit.log

check process jenkins_node matching "/opt/jenkins"
  start program = "/bin/bash /opt/jenkins_node/start.sh"
  if does not exist then restart

check process docker matching "/usr/bin/dockerd"
  start program = "/bin/systemctl start docker"
  if does not exist then restart
  if 5 restarts within 5 cycles then timeout

set httpd port 2812 and
    use address localhost  # only accept connection from localhost (drop if you use M/Monit)
    allow localhost        # allow localhost to connect to the server and
    allow admin:monit      # require user 'admin' with password 'monit'
```

## cmd
```Shell
# 查看监控状态：
monit status
# 重载配置：
monit reload
# 检测process matching配置：
monit procmatch "jensxxx"
# 停止监控某配置：
monit unmonitor namexx
# 开始监控某配置：
monit monitor namexx
```

## add to systemd
```Shell
vim /etc/systemd/system/monit.service
```
```Ini
[Unit]
Description=Monit Service
Documentation=https://mmonit.com/monit/documentation/monit.html
After=network.target

[Service]
ExecStart=/usr/local/bin/monit -Ic /etc/monitrc
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```
```Shell
systemctl daemon-reload
systemctl start monit
systemctl status monit
systemctl enable monit
```