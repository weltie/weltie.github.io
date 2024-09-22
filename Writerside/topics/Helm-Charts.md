# Helm-Charts

### install kubernetes

docker desktop enable kubernetes engine and check result:
```Shell
kubectl api-version
```

> **command not found**
>
> find kubectl file and add to env
>
{style="note"}

> **Error: INSTALLATION FAILED: Kubernetes cluster unreachable: Get "https://kubernetes.docker.internal:6443/version": EOF**
>
> add `127.0.0.1 kubernetes.docker.internal` to hosts file
>
{style="warning"}

### install helm (homebrew)

```Shell
brew install helm
```

### install helm (linux)

```Shell
1. Download your desired version https://github.com/helm/helm/releases
2. Unpack it (tar -zxvf helm-v3.0.0-linux-amd64.tar.gz)
3. mv linux-amd64/helm /usr/local/bin/helm
```

### add repo

```Shell
helm repo add bitnami https://charts.bitnami.com/bitnami
```

### update repo
```Shell
helm repo update 
```

### install
```Shell
helm install bitnami/mysql --generate-name
```

### show list
```Shell
helm list
helm uninstall xxx
```

### port-forward
```Shell
kubectl port-forward $POD_NAME 8080:80
```

### debug cmd
```Shell
helm hint
helm install --generate-name --dry-run --debug . 
```

### package
```Shell
helm package deis-workflow
helm install deis-workflow ./deis-workflow-0.1.0.tgz
```

### show helm install status
```Shell
helm status --show-resources [chart]
```


> **resources stay pending**
>
> maybe the pv or pvc is not found.
>
{style="warning"}


### use gitlab as helm repo
```Shell
1. install helm-push plugin
helm plugin install https://github.com/chartmuseum/helm-push

2. add repo
helm repo add --username <xxx> --password <token> <repo_name> https://gitlab.example.com

3. push chart.tgz file
helm cm-push xxx.tgz <repo_name>
```

## Helm Chart 简介和常用命令

[HelmChart官方文档](https://helm.sh/zh/docs/)

### 简介

HelmChart是k8s的包管理器，如linux下的yum，apt等。
可直接通过helm install 安装应用到k8s。

gitlab helm管理文档[https://docs.gitlab.com/ee/user/packages/helm_repository/]

### 常用命令

```Shell
# 新建项目
helm create chart_xxx_name

# 打包项目
helm package chart_xxx_path

# 推送上传到自建gitlab
# 添加源
helm repo add --username xxx --password xxx my-gitlab https://gitlab.example.com

# 推送到gitlab用到cm-push指令，需要先安装cm-push插件
helm plugin install https://github.com/chartmuseum/helm-push

# 推送
helm cm-push chart_xxx_name-0.1.0.tgz yyxx-gitlab
```


