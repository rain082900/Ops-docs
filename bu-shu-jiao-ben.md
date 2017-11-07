### 集群扩容

> 本章适用于centos7及7.3，内核版本为3.10.0-514.6.2.el7.x86\_64

随着越来越多的应用加入集群，必然要面临集群资源不足的情况，集群的横向扩展能力就显得尤为重要。

对于kubernetes来说，一台主机对应一个节点，集群可以使用的资源总量约等于集群所有主机资源总量之和，本章将介绍如何快速向集群添加主机。

##### 扩容之前需要准备

* 已经部署了kubernetes集群的主节点
* 一台可以使用的服务器（虚拟机）

##### 安装前准备

* 关闭防火墙
* 设置selinux为disable

* 与集群主节点时间同步

##### 安装docker并修改其存储驱动

##### 安装kubunetes从节点

在有外网环境下执行命令

`curl -sS`[`http://s3.c2cloud.cn/k8s-builder/install.sh`](http://s3.c2cloud.cn/k8s-builder/install.sh)`| sh -s {k8s-token}  {主机IP}  {主节点IP}`

无外网环境需要提前下载部署脚本和安装包，然后执行

```
sh install.sh {k8s-token}  {主机IP}  {主节点IP}
```

> 安装包下载地址 [http://s3.c2cloud.cn/k8s-builder/install.tar.gz](http://s3.c2cloud.cn/k8s-builder/install.tar.gz)
>
> 安装脚本下载地址 [http://s3.c2cloud.cn/k8s-builder/install.sh](http://s3.c2cloud.cn/k8s-builder/install.sh)
>
> {主机IP}指当前登录的服务器IP
>
> {主节点IP}指部署了K8S主节点服务器的IP
>
> {k8s-token}指部署完k8s主节点以后自动分配的允许接入集群token，可以通过在主节点执行命令 `kubectl -n kube-system get secret clusterinfo -o yaml | grep token-map | awk '{print $2}' | base64 -d | sed "s|{||g;s|}||g;s|:|.|g;s/\"//g;" | xargs echo` 查看



