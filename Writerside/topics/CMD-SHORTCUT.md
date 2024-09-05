# CMD-SHORTCUT

## cp exclude caches dir
```Shell
ls tar1 | grep -v caches | xargs -i cp -r tar1/{} ./tar1_bak
```

## tar exclude caches dir
```Shell
tar --exclude caches -zcvf tar2.tar.gz ./tar1
```

## gerx 生成正则表达是
```Shell
grex 'a' 'ab' 'abc'
```

## 当前目录下各文件夹大小
```Shell
du -h --max-depth=1
```

## 当前目录下所有目录及子目录大小
```Shell
du -h - .
```

## 当前目录下所有文件大小（排序）
```Shell
du -sh * | sort -nr
```

## 系统磁盘占用
```Shell
 df（英文全拼：disk free） 命令用于显示目前在 Linux 系统上的文件系统磁盘使用情况统计
 -l, --local：仅显示本地文件系统。
 -h, --human-readable：以人类可读的格式显示输出结果。
 
 df -lh
```

## 网络相关
```Shell
可ip 可域名
ping xx.xx.xx.xx

traceroute xx.xx.xx.xx

telnet xxx.xx.xx.xx 8080

-- mac
nc -vz -w 2 xxx.xxx.xx.xx 8080
```