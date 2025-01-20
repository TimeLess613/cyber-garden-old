---
tags:
  - IT/容器
  - IT/DevOps
---
Docker Engine：即Docker CE（Community Edition）。在某次更新后更名为此，区别于“Docker Desktop”。

## 安装

> [!NOTE] 仅为过去的项目笔记，最新信息参考[官方文档](https://docs.docker.com/engine/install/)。

```
# yum install -y yum-utils
# yum-config-manager --add-repo
https://download.docker.com/linux/centos/docker-ce.repo
# yum install -y docker-ce docker-ce-cli containerd.io
```

**启动&确认：**
```
# systemctl enable docker
# systemctl start docker

# docker --version
```

## 配置代理

docker是C/S模型，其使用docker命令与dockerd交互。

所以代理有两种。

- 为Docker客户端（Docker CLI）配置代理：使容器中的程序能够使用环境变量中的代理配置
	- [proxy/#configure-the-docker-client](https://docs.docker.com/network/proxy/#configure-the-docker-client)
- 为Docker守护程序（dockerd）本身配置代理服务：其他情况（如pull）
	- [systemd/#httphttps-proxy](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)

### dockerd

添加配置文件
```
# cat /etc/systemd/system/docker.service.d/http-proxy.conf
[Service]
Environment="HTTP_PROXY=http://proxy:8080"
Environment="HTTPS_PROXY=http://proxy:8080"
```

应用配置
```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

确认是否应用了配置

- `docker info`：会返回所有容器和镜像的基本信息
- `sudo systemctl show --property=Environment docker`



## .dockerenv文件

容器的根目录一般都有这个文件——渗透时可根据这个文件判断自己是否在容器中。



## docker命令

### run

`docker run -i -t [--name 容器名] <image> /bin/bash(不写命令则默认用Dockerfile里的CMD指令)`

- -it：交互模式，用什么交互呢——伪[[终端]]（创建交互式shell，退出shell则容器会停止）
- -d：detached mode。将命令作为dockerd的附属进程，创建在后台的守护进程（就不需要 -it了）。【此时命令不该指定bash，否则容器无法启动】

- 会先检查本地有无image，没有则联网官方Registry（Docker Hub）下载
- 容器创建后自动生成自己的网络，以及一个用来和宿主机通信的网桥
	- 安装Docker时会创建网络接口docker0，每个容器都会在docker0上分配一个IP。接口本身是网关。
		- 默认172.17.0.0/16。可修改[[docker#网桥默认池|网桥默认池]]
		- veth开头的是容器接口，默认连接在docker0上。

- `-p <port>`：容器公开哪个端口给外部（即宿主机），Docker随机选一个大端口映射给容器的port
	- 公开端口绑定到本地网络接口，可以将服务公开于宿主机所在的网络。
	- 不指定公开端口的话，容器将默认使用"bridge"网络模式，可与主机、容器间通信。
	- `-p <宿主ip:port>:<容器port>`

- -P：公开Dockerfile中EXPOSE指令设置的所有端口（容器的，会指向宿主机的随机端口）
- -v：volume。挂载主机目录，即主机上的xxx和容器中xxx目录是同步的。但容器销毁，宿主机目录仍会存在。

### 删除

- `docker container rm`：删除容器文件
- `docker image rm`：删除镜像文件


### 启动容器

- `start <容器>`
- `restart <容器>`

### 进入容器

#### exec

- `exec -it <容器> <命令>`

在容器内另外启动新进程（对比[[docker#attach|attach]]）。

使用`docker exec -it <容器> /bin/bash`进入容器内，**这种方法是最通用的方法**。（ver1.3之后；ver1.2之前用nsenter）

> [!NOTE] 请按`Ctrl+P+Q`进行退出容器
> 如果要正常退出不关闭容器（即使-d启动也），请按`Ctrl+P+Q`进行退出容器，这一点很重要，请牢记！

> [!NOTE] 容器的运行机制
> Docker容器在后台运行，必须要有一个前台进程（如shell），这里我们让容器有前台程序运行，就可以实现容器的`-d`启动后存活。（如果前台进程不是shell这种，而是运行`ps`的话，容器执行完命令后就会关闭）

#### attach

- `attach <容器>`

附着到容器正在执行的命令进程（docker ps看COMMAND）的终端。

> [!NOTE] 这个少用
> 启动命令不是sh的话进入容器会卡住


### 网络

> [!NOTE] --link 将被弃用
> 公告： [https://docs.docker.com/network/links/](https://docs.docker.com/network/links/)
> 对策：用[[docker#docker网络|docker网络]]功能（即bridge等那个，基本命令如下）

- `docker network ls`：查看本地docker主机上的全部网络
- 新建网桥：`docker network create my_network`
- 连接：`docker run --net my_network --name app`
	- 于是构建容器命令变为：`docker run --rm -itd --name app1 --expose 80 --net my_network php /bin/sh`


### 镜像

- 搜索：`search  <image>`
- 获取：`pull <image:tag (默认“:latest)”>`
- 查看：`docker image <CONTAINER>`
- 查看镜像被构建时的每一步：`docker history <IMAGE>`

#### 构建镜像

推荐：编写Dockerfile后用docker build命令。

构建环境的目录（即放Dockerfile文件的，称为context）会把context的目录和文件上传到Docker守护进程，使得守护进程能够直接访问你想在镜像中存储的任何代码、文件等。

- -t 命名，建议为镜像设置标签（默认为lateset）。最后的点"."告诉Docker到本地目录找Dockerfile文件。
- 第一条指令都是FROM，指定基础镜像（base image）
- 默认有构建缓存——构建失败的话可以进中途成功的容器继续构建
- （对于CMD？）推荐使用以字符串数组语法来设置要执行的命令。不用的话docker会在指定命令前加上`/bin/sh -c`，可能有意外
- RUN（容器构建时运行）可以覆盖CMD（容器启动时运行）。Dockerfile只指定一条CMD，就算有多条也只有最后一条被使用
- ADD 将context的文件复制到镜像中
- VOLUME 卷目录可以绕过联合文件系统，以正常文件形式存在于宿主机，提供容器间的数据共享和持久化功能。



### 改名

`docker rename UUID/oldname newname`

- UUID（Universally Unique Identifier）

### logs

`logs -f <容器>`

与tail -f相似。


### top

`top <容器>`




## 挂载


### 持久化

> 理解： [https://4sysops.com/archives/introduction-to-docker-bind-mounts-and-volumes/](https://4sysops.com/archives/introduction-to-docker-bind-mounts-and-volumes/)

#### bind方式（就算指定的是文件，也必定被当做目录）

- `-v <宿主机>:<容器>`
- 通过`docker inspect`命令，查看容器“Mounts”那一部分

#### 卷方式（就算指定的是文件，也必定被当做目录）

```
docker volume --help
docker volume create logdata
docker volume ls
docker run -it --name volume1 --mount type=volume,source=logdata,target=c:\logdata microsoft powershell
```

#### 区别

![[Pasted image 20240317223116.png]]
![[Pasted image 20240317223138.png]]



### 改映射

> [https://www.cnblogs.com/poloyy/p/13993832.html](https://www.cnblogs.com/poloyy/p/13993832.html)

![[Pasted image 20240317222529.png]]


 > [!NOTE] [但容器应该是短暂的](https://github.com/moby/moby/issues/31452)
 > 所以一般还是停止容器并新建run。
![[Pasted image 20240317222629.png]]




## docker网络

> [[虚拟网卡]]

- 不同网段的容器ping不通——可把容器加入多个网桥解决
- 默认网桥不支持容器名解析，而自定义网桥可以

![[Pasted image 20240317223340.png]]

 > [Docker容器间通信与外网通信](https://blog.csdn.net/vanvan_/article/details/98663379?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.essearch_pc_relevant&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.essearch_pc_relevant)
>  [Docker 容器网络访问原理，SNAT和DNAT](https://blog.csdn.net/shi_hong_fei_hei/article/details/114481494)



### 网桥默认池

> [https://straz.to/2021-09-08-docker-address-pools/](https://straz.to/2021-09-08-docker-address-pools/)

- **base**是整个docker网络
- **size**是从base中划分给每个bridge的大小
```
# cat /etc/docker/daemon.json
{
  "default-address-pools": [
    { "base": "10.1.0.0/12", "size": 24 }
  ]
}

# sudo systemctl restart docker
```
- 重启服务后，已有的bridge也会自动变更网段（目前只测试了默认bridge）


docker-compose 也可自定义（需要docker-compose.yml文件定义version2或以上）  
> [https://knowledge.sakura.ad.jp/26522/](https://knowledge.sakura.ad.jp/26522/)



## docker-compose

...










---
## 问题管理

### 构建docker的nginx后访问报403错误

解决：[关闭SELinux](onenote:..\20.渗透\环境.one#最小安装&section-id={4F53940D-9292-4939-8ED7-4F9EB86D1E96}&page-id={1F0F8C37-26AF-49F5-8C99-B5CD4E03A395}&object-id={F504FA59-1F0D-4CC7-959B-9BD5D23AE313}&18&base-path=https://d.docs.live.net/d10d79d0adccf558/文档/我的笔记本)。容器需要重开

### docker下overlay2占用空间过大

解决：清理docker占用空间

- [https://www.cnblogs.com/Henry121/p/15627488.html](https://www.cnblogs.com/Henry121/p/15627488.html)
- [https://www.creationline.com/lab/35518](https://www.creationline.com/lab/35518)



