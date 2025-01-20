---
tags:
  - IT/容器
  - IT/DevOps
---

## 概念

控制面板（control plane）

Node：宿主机。可以是裸机服务器或虚拟机。多个应用服务共享1个Node的计算资源。里面包含容器运行时组件（用于下载和部署镜像）。

Pod：k8s中最小的调度单位。里面放应用服务容器（如docker），一个应用服务容器一般还搭配有log或监控容器，所以这几个打包为一个pod当做最小的调度单位。当然也可以1pod1容器。




## 快捷

在`~/.bashrc`最后添加别名：`alias k="kubectl --namespace <your namespace>"`
- 但是似乎在sudo后无法接别名，所以可以将sudo也加入到上面的值中。
- --namespaceke


## 修改k3s服务单元

```bash
$ sudo systemctl daemon-reload
$ sudo systemctl restart k3s
$ sudo cat /etc/systemd/system/k3s.service
```

### 配置代理

```bash
$ sudo vi /etc/systemd/system/k3s.service.env
$ sudo systemctl restart k3s
```

## kubectl命令

用于与k8s的API（控制面板的API Server）交互。


### `kubectl cluster-info`




### `kubectl get nodes`



### `kubectl get services`





### `kubectl get pods -A`

- -A：不用-A则仅显示default命名空间的pods
- -n：指定命名空间






### `kubectl describe pods [NAME]`







### `kubectl logs <NAME>`









## 文章摘抄

> [来自小白debug](https://mp.weixin.qq.com/s?__biz=MzkxNTU5MjE0MQ==&mid=2247494562&idx=1&sn=2b4ff205e420dd9fbb2b40c95531b8a4&chksm=c15e6d00f629e41665384583ceb05710c05f34f62c531019d06d35b6659b95a5b7deb3ef3216&mpshare=1&scene=23&srcid=0309ArlG6gobNwP1srrsMgFY&sharer_shareinfo=f39fec113018a4a33b79ad5aafc11049&sharer_shareinfo_first=f39fec113018a4a33b79ad5aafc11049#rd)
> [小白debug B站视频](https://www.bilibili.com/video/BV1Du4m137pK)


![[Pasted image 20240313094710.png]]
![[Pasted image 20240313094734.png]]

### 控制面板内部组件

![[Pasted image 20240313095010.png]]

![[Pasted image 20240313095111.png]]

### 服务部署流程

![[Pasted image 20240313095337.png]]

