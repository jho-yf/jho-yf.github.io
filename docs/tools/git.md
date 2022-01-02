# Git

## Git小技巧

### git设置代理

```sh
# 全局代理
git config --global http.proxy 127.0.0.1:1087
# 局部代理
git config --local http.proxy 127.0.0.1:1087

# 查询全局代理
git config --global http.proxy
# 查询局部代理
git config --local http.proxy

# 取消全局代理
git config --global --unset http.proxy
# 取消局部代理
git config --local --unset http.proxy
```



### git克隆

```sh
# 下载指定的分支tag代码
git clone --branch [tags] [git地址]
git clone -b [tags] [git地址]
git clone -b 2.2.0 git@github.com:xuxueli/xxl-job.git
```



### git分支

```sh
# 查看本地分支,在输出结果中，前面带*的是当前分支
git branch
# 查看远端库的分支情况
git branch -r
# 查看远程和本地所有分支
git branch -a

# 从master创建新的分支dev_jho
git checkout -b dev_jho
# 基于dev_xxx分支创建新的分支
git checkout -b dev_jho origin/dev_xxx

# 建立本地分支到远程分支的映射关系，才能提交代码
## 方法1
git branch --set-upstream-to=origin/dev_jho
## 方法2
git branch -u origin/dev_jho
# 撤销本地分支与远程分支的映射关系
git branch --unset-upstream

## 查看本地分支和远程分支的映射关系
git branch -vv
```



### 删除已经提交的文件

```sh
# 添加到.gitignore文件

# 删除远程文件
git rm -r --cached .idea/*
git rm -r --cached target/*

# 提交
git commit -m "删除不需要的文件"
git push
```