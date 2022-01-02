# Docker
## 概述

### 相关资料

- 官方文档：https://docs.docker.com/

- dockerhub：https://hub.docker.com/
- docker离线包：https://download.docker.com/
- docker-compose离线包：https://github.com/docker/compose/releases
- 菜鸟教程：https://www.runoob.com/docker/docker-tutorial.html

### Docker和虚拟机技术的区别
传统虚拟机：虚拟出硬件，运行一个完整的操作系统，然后在操作系统上安装和运行软件

Docker：
* 容器内的应用直接运行在宿主机内，容器没有自己的内核，也没有虚拟硬件
* 每个容器间是相互隔离，每个容器内都有一个属于自己的文件系统，互不影响

### Docker为什么比虚拟机快

- Docker有比虚拟机更少的抽象层
- Docker利用的是宿主机的内核，vm需要多的是Guest OS

### Docker和传统的部署方式的区别
- 应用更快速的交付和部署

  * 传统：一堆帮助文档，安装程序

  * Docker：打包镜像发布测试，一键运行

- 更便捷的升级和扩缩容

- 更简单的系统运维
  * 在容器化之后，开发、测试环境都是高度一致的

- 更高效的计算资源利用
  * Docker是内核级别的虚拟化，可以在一个物理虚拟机上运行很多容器实例，服务器性能可以压榨到极致

## Docker的基本组成
![001Docker架构图.png](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202201022202090.png)

**镜像（image）**：docker镜像就像一个模板，通过这个模板能够创建多个容器（最终服务运行或者项目运行就是在容器之中）

**容器（container）**：Docker利用容器技术，独立运行一个或者一组应用。容器通过镜像来创建

* 可以把这个容器理解为一个简单的linux系统

**仓库（repository）**

* DockerHub
* 阿里云

## Docker安装
> 参考：
>
> - https://docs.docker.com/engine/install/ubuntu/
> - https://www.runoob.com/docker/ubuntu-docker-install.html

判断是否安装成功

```
docker version
```

hello world

```
docker run hello-world
```

### 卸载Docker
卸载Docker引擎、客户端和容器包

```
sudo apt-get purge docker-ce docker-ce-cli containerd.io
```

删除所有镜像、容器和数据卷

```
sudo rm -rf /var/lib/docker
```

### 阿里云镜像加速
参考阿里云容器镜像服务 > 镜像加速器

![002阿里云镜像加速器.png](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202201022202091.png)



## Docker工作原理

* Docker是一个Client-Server结构的系统，Dockers的守护进程运行在主机上，通过Socket与客户端通讯
* DockerServer接收到Docker-Client的指令，就会执行这个命令
![003Docker工作原理.png](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202201022202092.png)

2. 

## Docker常用命令

> 帮助文档：https://docs.docker.com/reference/

![004Docker命令小结.png](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202201022202093.png)

### 启动docker
	systemctl start docker
	systemctl restart docker
	systemctl stop docker
### 帮助命令
	docker version 			# 显示docker版本信息
	docker info				# 显示docker的系统信息，包括镜像和容器的数量
	docker 命令 --help		# 万能命令
### 镜像命令
	docker images			# 查看所有本地的主机上的镜像
		* 可选项
			* -a,--all		# 查看所有
			* -q,--quiet	# 只显示镜像id
	docker search xxxx		# 搜索镜像
		* 可选项
			* --filter=STARS=3000	# 筛选STARS未3000以上的
	docker pull xxx[:tag]			# 指定版本下载镜像
	docker pull xxx					# 默认下载最新镜像
	docker rmi -f imagesid			# 删除指定容器
	docker rmi -f imagesid1 imagesid2	# 批量删除指定容器
	docker rmi -f $(docker images -aq)	# 删除全部容器

> root@iZ2ypbthtz7121Z:~# docker pull mysql
> Using default tag: latest				# 如果不指定，默认就是latest，最新版
> latest: Pulling from library/mysql	
> 6ec8c9369e08: Pull complete 			# 分层下载，docker image的核心 联合文件系统
> 177e5de89054: Pull complete 
> ab6ccb86eb40: Pull complete 
> e1ee78841235: Pull complete 
> 09cd86ccee56: Pull complete 
> 78bea0594a44: Pull complete 
> caf5f529ae89: Pull complete 
> cf0fc09f046d: Pull complete 
> 4ccd5b05a8f6: Pull complete 
> 76d29d8de5d4: Pull complete 
> 8077a91f5d16: Pull complete 
> 922753e827ec: Pull complete 
> Digest:sha256:fb6a6a26111ba75f9e8487db639bc5721d4431beba4cd668a4e922b8f8b14acc
> Status: Downloaded newer image for mysql:latest
> docker.io/library/mysql:latest		# 真实地址

### 容器命令
	docker run [可选参数] image		# 新建并运行容器
		* 参数说明
			* --name="Name"			# 容器名字
			* -d					# 后台方式运行
			* -it					# 使用交互方式运行，进入容器查看内容
			* -p					# 指定容器的端口
				* -p ip:主机端口:容器端口
				* -p 主机端口:容器端口
				* -p 容器端口
	        * -P					# 随机指定端口
	        * -net					# 指定docker 网络

### 列出所有运行的容器
	docker ps [可选参数]				# 列出当前正在运行的容器
		* -a						# 列出当前正在运行的容器+历史运行过的容器
		* -n=?						# -n=2 列出最近创建的2的容器
		* -q						# 只显示容器的编号
### 退出容器
	exit							# 退出并停止容器
	ctrl + p + q					# 退出不停止容器

### 删除容器
	docker rm 容器1id	容器2id				# 删除指定容器，不能删除正在运行的容器
	docker rm -f 容器1id 容器2id				# 强制删除指定容器
	docker rm -f $(docker ps -aq)		# 删除所有容器
	docker ps -a -q|xargs docker rm		# 删除所有容器

### 启动和停止容器的操作
	docker start 容器id		# 启动容器
	docker restart 容器id		# 重启容器
	docker stop 容器id		# 停止当前正在运行的容器
	docker kill 容器id		# 强制停止当前容器

### 常用其他命令
#### 后台启动容器

* docker容器使用后台运行就必须要有个前台进程，docker发现没有应用就会自动停止

		docker run -d centos

#### 查看日志
	docker logs -tf --tail 10 容器id
	* -tf					# 显示日志
	* --tail number			# 要显示日志条数

#### 查看容器中进程信息
	docker top 容器id

#### 查看容器的元数据（配置信息）
	docker inspect 容器id

#### 进入当前正在运行的容器
	docker exec -it 容器id bashShell		# 进入容器后，开启一个新的终端，可以在里面操作
	docker attach 容器id					# 进入容器正在执行的终端，不会启动新的进程

#### 从容器内拷贝文件到主机
	docker cp 容器id:容器内路径 目的主机路径

#### 查看docker CPU的状态
	docker stats 容器id					# 查看特定容器的CPU状态
	docker stats							# 查看所有容器的CPU状态

#### 查看docker镜像更变历史

```shell
docker history 镜像id
```

#### network相关

```shell
# 查看docker网络地址
docker network ls
```



## 可视化

* portainer
* Rancher

### portainer
Docker图形化界面管理工具

安装	

```shell
docker run -d -p 8088:9000 --restart=always -v /var/run/docker.sock --privileged=true portainer/portainer

# -v 挂载，将容器数据挂载到本机
# -privileges=true 授权
```

访问：http://localhsot:8088



## Docker镜像

### 镜像是什么
镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置环境。

### 联合文件系统（UnionFS）

* UnionFS：Union文件系统是一种分层、轻量级且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层叠加，同时可以将不同目录挂载到同一虚拟文件系统之下。
* 特性：一次同时加载多个文件系统，但是从外部看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加，最终的文件系统会包含所有底层的文件和目录
* Union文件系统是Docker镜像的基础。镜像可以通过分层来继承，基于基础镜像，可以制作各种具体的应用镜像。

### docker镜像加载原理
- Docker镜像实际上由一层一层的文件系统组成的，这种层级文件系统就是UnionFS

- **bootfs(boot file system)**：在docker镜像的最底层，主要包含bootloader和kernel。

  * bootloader：用于引导加载kernel

  * kernel：内核

* 典型的Linux和Unix系统：包含boot加载器和内核
* 当boot加载完成之后整个内核就在内存中，此时内存的使用权由bootfs转交给内核，系统会卸载bootfs

- rootfs(root file system)：在bootfs的上一层，包含典型Linux系统中的/dev，/proc，/bin，/etc等标准目录和文件。对于一个精简的OS，rootfs只需包含基本命令，工具和程序即可
  ![005Docker镜像加载原理.png](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202201022202094.png)

### docker镜像分层
Docker镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像顶部，这一层就是容器层，容器层之下都是镜像层

### commit镜像
`docker commit`提交一个容器成为一个新的镜像。如果想保存当前容器的状态，就可以通过commit来提交，来生成一个新的镜像副本

```shell
docker images -m="提交的描述信息" -a="作者" 容器id 目标镜像名:[TAG]
```



## 数据卷

> 如果数据都存储与容器中，那么容器删除，数据就会丢失
> ==需求：数据可以持久化==

* 容器之间可以有一个数据共享的技术，Decker容器中产生的数据，同步到本地，这就是卷技术
* 目录的挂载，将我们容器内的目录，挂载到主机上面
* ==*容器的持久化和同步操作，容器间的数据共享*==

### 使用数据卷
#### 使用命令的方式挂载 `-v`

```shell
docker run -it -v 主机目录:容器内目录 镜像名称|ID /bin/bash
docker run -it -v /home/test:/home centos /bin/bash

# 查看容器详细信息，可查看挂载信息
docker inspect centos
```

![006Docker目录挂载.png](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202201022202095.png)

#### 匿名挂载和具名挂载
**匿名挂载**：挂载数据卷的时候，-v只指定容器内路径，不指定容器外路径

```shell
# 匿名挂载
docker run -it -v 容器内目录 镜像名称|ID	
# 查看所有volume的情况
docker volume ls			

# DRIVER              VOLUME NAME
# local               b4cc8cd20d274f0b771742232dd0468a0aa4d6c4d2aee1828d483d468c8d54d8
# local               ff710ca2d0e6d555ed8e85c7f37f9988801c590e61fabf4b66965e40dafea254
```

**具名挂载**：挂载数据卷的时候，-v指定数据卷名称和容器内部路径

```shell
# 具名挂载
docker run -it -v 数据卷名称:容器内路径 镜像名称|ID

# 查看所有volume的情况
docker volume ls

# DRIVER              VOLUME NAME
# local               nginx-volume

# 查看某个数据卷的详细信息
docker volume inspect 数据卷名称		
 
# [
#     {
#         "CreatedAt": "2020-08-16T08:21:13+08:00",
#         "Driver": "local",
#         "Labels": null,
#         "Mountpoint": "/var/lib/docker/volumes/nginx-volume/_data",
#         "Name": "nginx-volume",
#         "Options": null,
#         "Scope": "local"
#     }
# ]
```

* docker容器内的数据卷，在没有指定主机目录的情况下都默认在存放在`/var/lib/docker/volumes/xxxx/_data`

* 通过具名挂载可以更方便的找到卷，大多数情况使用具名挂载

* 可以改变数据卷的读写权限
	
	**ro（readonly）**：只读，这个路径内容只能通过主机来操作，容器内部无法操作
	**rw（readwrite）**：可读可写
  
  ```shell
  # -v 数据卷名称|主机路径:容器内路径:ro|rw
  docker run -d -P --name nginx01 -v nginx-volume:/etc/nginx:ro|rw nginx
  ```

### 数据卷容器
实现容器间的数据共享

```shell
docker run -it --name 容器1 --volumes-from 父容器 镜像id
```

数据卷容器的生命周期一直持续到没有容器使用为止，一旦持久化到本机，本机数据不会删除



## DockerFile

Dockerfile是用于构建docker镜像的文件！命令参数脚本

### Docker镜像的构建步骤

1. 编写dockerfile文件
2. docker build 构建成为一个镜像
3. docker run 运行镜像
4. docker push 发布镜像（DockerHub、阿里云镜像仓库）

### DockerFile的构建

- 每个保留关键字都是大写的

- 执行顺序从上到下

- 符号#表示注释

- 每个指令都会创建提交一个新的镜像层

### DockerFile的指令

```shell
FROM			# 基础镜像，一切从这里开始构建
MAINTAINER		# 镜像是谁写的，姓名＋邮箱
RUN				# 镜像构建的时候需要运行的命令
ADD				# 添加内容，会自动解压
WORKDIR			# 镜像工作目录
VOLUME			# 挂载的目录
EXPOSE			# 暴露端口
CMD				# 指定容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT		# 指定容器启动的时候要运行的命令，可以追加命令
ONBUILD			# 当构建一个被继承DockerFile时，就可以运行ONBUILD触发指令
COPY			# 类似ADD，将文件拷贝到镜像中
ENV				# 构建的时候设置环境变量
```

![image-20210205151400547](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202201022202098.png)

- CMD和ENTRYPOINT的区别
- ADD和COPY的区别



## Docker网络

- 容器和容器之间是可以ping通的
- Docker中的所有网络接口都是虚拟的，转发效率高
- 只要容器删除，对应网桥就没了

### docker0

**docker默认网络**，域名不能访问，使用`--link`可以打通

### 网络模式

**bridge**：桥接docker 默认

**none**：不配置网络

**host**：和宿主机共享网络

**container**：容器网络连通

### 网络连通

把一个容器连接到一个网络上

```shell
docker network connect [OPTIONS] NETWORK CONTAINER
```



## Docker-compose

* 定义、运行多个容器
* 批量容器编排



更新中...

