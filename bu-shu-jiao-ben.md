### 部署脚本

> 本部署脚本只适用于centos7及7.3，内核版本为3.10.0-514.6.2.el7.x86\_64

#### 部署k8s主节点

curl -sS [http://s3.c2cloud.cn/k8s-builder/install-master.sh](http://s3.c2cloud.cn/k8s-builder/install-master.sh) \| sh -s  {主机IP}

#### 部署K8S从节点（计算节点）

curl -sS [http://s3.c2cloud.cn/k8s-builder/install.sh](http://s3.c2cloud.cn/k8s-builder/install.sh) \| sh -s {k8s-token}  {主机IP}  {主节点IP}

> {主机IP}指当前登录的linux虚拟机（物理机）IP
>
> {主节点IP}指部署了K8S主节点虚拟机（物理机）IP
>
> {k8s-token}指部署完k8s主节点以后自动分配的允许接入集群token，可以通过命令 `kubectl -n kube-system get secret clusterinfo -o yaml | grep token-map | awk '{print $2}' | base64 -d | sed "s|{||g;s|}||g;s|:|.|g;s/\"//g;" | xargs echo` 查看



