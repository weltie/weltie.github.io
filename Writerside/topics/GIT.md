# GIT

## .git目录清理 https://bbs.huaweicloud.com/blogs/343828

### 重建仓库
重建仓库的这种做法，算是一种比较一劳永逸且相对而言比较简单的方式。既然现在的仓库已经让我们无法忍受，与其这样，还不是删除重建来的爽快。但是，这种做法一般情况下，都是不可行，除非是自己的本地项目。

### 删除大文件
直接找到 .git 目录下的大文件，将其删除掉，之后推送到远程代码库里面。这样做的前提是，删除所有其他分支，保留 master 或者 main 分支。

```Shell
# 查找大文件
$ git verify-pack -v .git/objects/pack/*.idx
12235d...dewaaaa34 tree   135 137 144088922
a453ab...34se212qz blob   3695898 695871 144734158
......

# 筛除前五个且保留第一列
$ git verify-pack \
    -v .git/objects/pack/*.idx | \
    sort -k 3 -n | tail -5 | awk '{print$1}'
12q626a...23a3
2z32ax1...azfd
......

# 查找出最大的5个文件和对应Commit信息
$ git rev-list --objects --all | \
    grep "$(git verify-pack -v .git/objects/pack/*.idx | \
    sort -k 3 -n | tail -5 | awk '{print$1}')"
91266a...sdfa3    data/xxx.pkl
232ax1...acafd    data/yyy.pkl
......

# rev-list:    列出Git仓库中的所有提交记录
# --objects:   列出该提交涉及的所有文件ID
# --all:       所有分支的提交(位于/refs下的所有引用)

# 将其删除掉
$ git filter-branch \
    --force --prune-empty --index-filter \
    "git rm -rf --cached --ignore-unmatch YOU-FILE-NAME" \
    --tag-name-filter cat -- --all

# filter-branch:  重写Git仓库中的提交
# --index-filter: 指定后面命令进行删除
# --all:          所有分支的提交(位于/refs下的所有引用)

# 强制推送
$ git push --force --all

# 彻底清除
$ rm -rf .git/refs/original/
$ git reflog expire --expire=now --all
$ git gc --prune=now
```

### GIT LFS MIGRATE
```Shell
# 重写master分⽀
# 将历史提交(指的是.git目录)中的*.zip都⽤lfs进⾏管理
$ git lfs migrate import --include-ref=master --include="*.zip"

# 重写所有分⽀及标签
# 将历史提交(指的是.git目录)中的*.rar,*.zip都⽤lfs进⾏管理
$ git lfs migrate import --everything --include="*.rar,*.zip"

# 切换后需要把切换之后的本地分支提交到远程仓库了，需要手动push更新远程仓库中的各个分支
$ git lfs push --force

# 切换成功后，GIT仓库的大小可能并没有变化
# 主要原因可能是之前的提交还在，因此需要做一些清理工作
# 如果不是历史记录非常重要的仓库，建议不要像上述这么做，而是重新建立一个新的仓库
$ git reflog expire --expire-unreachable=now --all
$ git gc --prune=now

```

### GIT LFS
```Shell
# 1.开启lfs功能
$ git lfs install

# 2.追踪所有后缀名为“.psd”的文件
$ git lfs track "*.iso"

# 3.追踪单个文件
git lfs track "logo.png"

# 4.提交存储信息文件
$ git add .gitattributes

# 5.提交并推送到GitHub仓库
$ git add .
$ git commit -m "Add some files"
$ git push origin master
```