# Docker

### Uninstall old versions
```Shell
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### Set up the repository
```Shell
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

### install the latest version
```Shell
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### install a specific version
```Shell
yum list docker-ce --showduplicates | sort -r

docker-ce.x86_64    3:27.1.1-1.el9    docker-ce-stable
docker-ce.x86_64    3:27.1.0-1.el9    docker-ce-stable
<...>


sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io docker-buildx-plugin docker-compose-plugin
```

### Start Docker.
```Shell
sudo systemctl start docker
```

### Verify that the Docker Engine
```Shell
sudo docker run hello-world
```

### multi platform
```Shell
docker buildx build --platform linux/amd64,linux/arm64 .
```
