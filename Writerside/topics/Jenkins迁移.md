# Jenkins迁移

## 备份
### 将 var/jenkins_home 打包备份，其中caches目录可以过滤，文件较大
```Shell
tar --exclude caches -zcvf jenkins_home.tar.gz ./jenkins_home
```
> 备份前可以的话先关闭服务
> 避免压缩途中数据丢失
> 
> 或者先备份目录，再打包
> 
### [可选]将/usr/share/jenkins/jenkins.war打包备份 

## 还原
放到新机器上解压即可



## 异常汇总

### 解压文件报错
gzip: stdin: not in gzip format
> 网络问题，用sz从机器上下载时可能会丢包，导致文件异常，重新下载后解决
> {style="warning"}

### 登录镜像仓库报错
docker login net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
> 访问权限问题，加白名单后解决
> {style="warning"}

### 代码检出自建gitlab卡住
git fetch --tags --progress xxx +refs/heads/*:refs/remotes/origin/* # timeout=10
> 网络问题，更换自建gitlab公网ip后解决(绑定公网ip时有时地域距离过大，丢包严重容易产生该异常)
> {style="warning"}