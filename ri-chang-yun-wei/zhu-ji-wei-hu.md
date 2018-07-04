#### **1.定期查看主机资源使用情况（内存，磁盘）**

查看内存使用情况命令：

`free -m`

![](/assets/24.png)

如果Mem一栏 对应的free值小于1000，说明内存不够了,排查下当前系统所有进程内存占用情况

linux下获取占用内存资源最多的10个进程命令:

`ps aux|head -1;ps aux|grep -v PID|sort -rn -k +4|head`![](/assets/26.png)查看这些进程是否为正常应用进程，如果为正常进程，此时申请增加主机内存；

如果为非正常进程，结束掉该进程，使用命令

`kill -9 $PID`

然后在重复 查看内存使用情况命令，核查内存使用情况.

查看磁盘空间命令: 

`df -h`

![](/assets/28.png)

Avail 一栏值如果小于5G,说明该目录磁盘空间不足了

#### **2.查看docker kubelet进程情况**

查看docker日志信息: systemctl status docker -l

查看docker kubelet进程情况:systemctl status  kubelet  -l

#### **3.资源不足的处理方法**

a.执行清理脚本

* \#清理docker日志
* \#!/bin/sh
* echo "==================== start clean docker containers logs =========================="
* logs=$\(find /var/lib/docker/containers/ -name \*-json.log\)
* for log in $logs
* ```
      do  

              echo "clean logs : $log"  

              cat /dev/null &gt; $log  

      done
  ```
* echo "==================== end clean docker containers logs   =========================="
* \#清理已经停止或者不再使用的docker资源，包括
* \#- all stopped containers
* \#- all volumes not used by at least one container
* \#- all networks not used by at least one container
* \#- all dangling images
* docker system prune
* \#删除所有已停止的容器：
* docker rm $\(docker ps -a -q\)
* \#清理未在使用的存储卷：
* docker volume ls -q \| xargs -r docker volume rm

b. 申请更多资源

申请增加主机内存、cpu额度

c. 主机加入集群

参照科创大平台部署文档之部署计算（node）节点

#### **4.定期重启主机**

shutdown -r now

