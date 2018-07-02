1。定期查看主机资源使用情况（内存，磁盘）

查看内存使用情况：free -g

查看磁盘空间: df -h

2。查看docker kubelet进程情况

查看docker日志信息: systemctl status docker -l

查看docker kubelet进程情况:systemctl status  kubelet  -l

3。资源不足的处理方法

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



4。定期重启主机

shutdown -r now

