# K8S

`kubectl get namespace`

新建一个名为 my-namespace.yaml 的 YAML 文件，并写入下列内容：
```
apiVersion: v1
kind: Namespace
metadata:
name: <insert-namespace-name-here>
```
然后运行：
```
kubectl create -f ./my-namespace.yaml
```
或者，你可以使用下面的命令创建名字空间：
```
kubectl create namespace <insert-namespace-name-here>
```

### delete namespace
`kubectl delete namespaces <insert-some-namespace-name>`

### get resources
`kubectl get pods -n my_namespace`

`kubectl describe pod <pod-name>`

### top(need Metrics Server)
`kubectl top pod <pod-name>`