# Docker-Proxy

https://docs.docker.com/config/daemon/proxy/#environment-variables
```Shell
sudo mkdir -p /etc/systemd/system/docker.service.d
vim /etc/systemd/system/docker.service.d/http-proxy.conf
```

`http-proxy.conf` file content

```Shell
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:3128"
Environment="HTTPS_PROXY=https://proxy.example.com:3129"
```

```Shell
sudo systemctl daemon-reload
sudo systemctl restart docker

sudo systemctl show --property=Environment docker
```
