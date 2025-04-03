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

### space used
```Shell
docker system df

docker system df -v 
```

### cache prune
```Shell
# 清理磁盘，删除关闭的容器、无用的数据卷和网络，以及dangling镜像（即无tag的镜像）。
docker system prune

# 清理得更加彻底，可以将没有容器使用Docker镜像都删掉。
docker system prune -a

# 保留最近10天的缓存示例命令如下
docker builder prune --filter 'until=240h'
```

### docker desktop 
starting vpnkit-controller and storage-provisioner
```Shell
rm -rf ~/.kube
```

### 删除 none 镜像
```Shell
docker images | grep none | awk '{print $3}' | xargs docker rmi

docker images | grep none | awk '$1 == "<none>" {print $1, $3}' | xargs docker rmi -f
```