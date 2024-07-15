# Jenkins-Node

## 启动方式选用Java Web
![Jenkins-Node-Configure.png](Jenkins-Node-Configure.png)

## 在节点机器上执行命令
选择Java Web方式的节点配置中会自动生成指令
```Shell
    java -jar agent.jar \
    -url http://my.jenkins.com:31316/ \
    -secret xx \
    -name "my-node" \
    -tunnel xxx.xxx.xx:50010
```
`注意-tunnel选项, 默认管理端口是50000, 示例中Jenkins搭建方式是Docker, 搭建时50000端口映射为50010, 所以需要-tunnel参数指定`