# Git

## Git工作流

![Git工作流](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202201082233643.png "Git工作流")

**`master`分支：**主要用于跟线上系统保持一致，所有上线的分支都是从`master`出来的，一般情况下开发人员是碰触不到的，主要由项目经理和运维人员维护。

**`hotfix`分支**：若`master`分支突然出现BUG，可以由`master`创建临时分支`hotfix`，用于紧急解决BUG。解决之后合并到`master`分支再进行上线。然后合并至`develop`分支后即可删除该临时分支。

**`develop`主线开发分支**：主要由开发人员操作，一般也不在此分支上进行开发工作，而是新建`feature_xxx`开发分支。

**`feature_xxx`开发分支**：多个开发分支可以互相不影响，并行开发多个模块。当`feature_xxx`分支开发完成之后，将该分支合并至`develop`主线开发分支，之后由`develop`分支新建`release.x.x`分支，进行测试。

**`release.x.x`测试分支**：如果测试出现BUG，直接再此分支上进行`fix`操作修复BUG，再进行测试，直到没有问题了，再合并到`master`主分支并上线，再合并到`develop`主线开发分支上。

> `master`分支和`develop`主线开发分支需要总是保持一致





## Git命令

### git克隆

```bash
# 下载指定的分支tag代码
git clone --branch [tags] [git地址]
git clone -b [tags] [git地址]
git clone -b 2.2.0 git@github.com:xuxueli/xxl-job.git
```



### git分支

查看分支

```bash
# 查看本地分支,在输出结果中，前面带*的是当前分支
git branch
# 查看远端库的分支情况
git branch -r
# 查看远程和本地所有分支
git branch -a
```

创建分支

```bash
# 从master创建新的分支dev_jho
git checkout -b dev_jho
# 基于dev_xxx分支创建新的分支
git checkout -b dev_jho origin/dev_xxx
```

关联本地分支和远程分支：建立本地分支到远程分支的映射关系，才能提交代码

```sh
## 方法1
git branch --set-upstream-to=origin/dev_jho
## 方法2
git branch -u origin/dev_jho
```

查看本地分支和远程分支的映射关系

```bash
git branch -vv
```

撤销本地分支与远程分支的映射关系

```bash
git branch --unset-upstream
```



### git设置代理

```bash
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



## Git小技巧

### 删除已经提交错文件

1. 添加到.gitignore文件，将需要相应的文件
2. 删除远程文件

```bash
git rm -r --cached .idea/*
git rm -r --cached target/*
```

3. 提交

```sh
git commit -m "删除不需要的文件"
git push
```