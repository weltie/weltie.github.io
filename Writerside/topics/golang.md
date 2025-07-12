# golang


go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.io,direct
go env -w GOPROXY=https://goproxy.cn,https://goproxy.io,direct

国内源
```Shell
echo "export GO111MODULE=on" >> ~/.profile
echo "export GOPROXY=https://goproxy.cn" >> ~/.profile
source ~/.profile
```




go mod init xxx
go mod tidy
go build

gvm list