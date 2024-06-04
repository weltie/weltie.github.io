# Jenkins-SSH-Node-Env-Config

## Step1.Install docker-engine

### uninstall old version
```
sudo yum remove docker \
docker-client \
docker-client-latest \
docker-common \
docker-latest \
docker-latest-logrotate \
docker-logrotate \
docker-engine
```
### set repository

```
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo \ 
https://download.docker.com/linux/centos/docker-ce.repo
```
### install
```
sudo yum install docker-ce docker-ce-cli containerd.io \ 
docker-buildx-plugin docker-compose-plugin
```
### start docker
```
sudo systemctl start docker
```
### test docker
```
sudo docker run hello-world
```
## Step2. JRE Config
```
tar -zxvf jre-8uversion-linux-x64.tar.gz
```
## Step2. Git Install
```
yum install git
```
