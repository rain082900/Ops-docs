### 主机巡检

当集群运行一段时间以后，主机的资源使用情况会发生变化，因此需要制定主机巡检计划，比如每两周检查主机的内存，磁盘，cpu资源是否满足应用正常运行的要求，如果发现资源不足，需要及时处理，保证应用运行环境稳定。

##### **定期查看主机资源使用情况（内存，磁盘,CPU）**

###### 查看内存使用情况命令：

```
free -h
```

![](/assets/24.png)

如果发现可用内存超过了总内存的90%，需要考虑迁移应用或者[集群扩容](/bu-shu-jiao-ben.md)

> 应用迁移请参见部署平台使用手册，集群扩容请参见[集群扩容](/bu-shu-jiao-ben.md)章节

##### 查看cpu使用情况

linux 下 取进程占用 cpu 最高的前10个进程命令：

```
ps aux|head -1;ps aux|grep -v PID|sort -rn -k +3|head
```

![](/assets/33.png)如果发现某进程的CPU占用长时间超过100%，请联系相关开发人员确认问题。

##### 查看磁盘空间命令:

```
df -h
```

![](/assets/28.png)

如果可用,执行下面的第3步--磁盘空间不足处理方法.

#### **2.查看docker kubelet进程情况**

##### 查看docker日志信息:

`systemctl status docker`

如果命令一致卡着不动，超过5分钟；判断docker进程已死,执行重启docker命令:

`systemctl restart docker`

然后在执行

`systemctl status docker`

正常状态显示![](/assets/31.png)

##### 查看docker kubelet进程情况:

`systemctl status  kubelet`

正常状态

![](/assets/32.png)

#### **3.资源不足的处理方法**

##### a.磁盘空间不足.

执行清理脚本

* `#清理docker日志`
* `#!/bin/sh`
* `echo "==================== start clean docker containers logs =========================="`
* `logs=$(find /var/lib/docker/containers/ -name *-json.log)`
* `for log in $logs`
* ```
      do  

              echo "clean logs : $log"  

              cat /dev/null &gt; $log  

      done
  ```
* `echo "==================== end clean docker containers logs   =========================="`
* `#清理已经停止或者不再使用的docker资源，包括`
* `#- all stopped containers`
* `#- all volumes not used by at least one container`
* `#- all networks not used by at least one container`
* `#- all dangling images`
* `docker system prune`
* `#删除所有已停止的容器：`
* `docker rm $(docker ps -a -q)`
* `#清理未在使用的存储卷：`
* `docker volume ls -q | xargs -r docker volume rm`

##### b. 申请更多资源

申请增加主机内存、cpu额度

##### c. 主机加入集群

参照科创大平台部署文档之部署计算（node）节点

#### **4.定期重启主机**

shutdown -r now

