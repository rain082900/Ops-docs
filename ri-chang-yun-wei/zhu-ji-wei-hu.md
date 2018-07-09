### 主机巡检

当集群运行一段时间以后，主机的资源使用情况会发生变化，因此需要制定主机巡检计划，比如**每两周**检查主机的内存，磁盘，cpu资源是否满足应用正常运行的要求，如果发现资源不足，需要及时处理，保证应用运行环境稳定。

##### 查看主机内存使用情况

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

![](/assets/33.png)  
如果发现某进程的CPU占用长时间超过100%，如果是应用进程请联系相关开发人员确认问题，如果是其他进程请联系云平台管理员

##### 查看主机磁盘空间

```
df -h
```

![](/assets/28.png)

如果根目录可用磁盘空间小于90%,请执行清理脚本clean\_disk.sh,内容如下：

```
#清理docker日志
#!/bin/sh  

echo "==================== start clean docker containers logs =========================="  

logs=$(find /var/lib/docker/containers/ -name *-json.log)  

for log in $logs  
        do  
                echo "clean logs : $log"  
                cat /dev/null > $log  
        done  


echo "==================== end clean docker containers logs   =========================="  

#清理已经停止或者不再使用的docker资源，包括
#- all stopped containers
#- all volumes not used by at least one container
#- all networks not used by at least one container
#- all dangling images
docker system prune

#删除所有已停止的容器：
docker rm $(docker ps -a -q)
#清理未在使用的存储卷：
docker volume ls -q | xargs -r docker volume rm
```

执行脚本

```
sh clean_disk.sh
```

如果此时根目录可用磁盘空间还是小于10%，请联系云平台管理员磁盘扩容

> 注意:clean\_disk.sh会删除该主机下所有容器的日志文件，如果有特殊要求，请在执行脚本之前保存日志

##### 检查docker进程

查看docker日志信息

```
systemctl status docker
```

查看主机上运行的容器情况

```
docker ps
```

> 在某些极端的情况下，执行docker ps没有响应，此时docker进程可能被卡死,但是运行状态下的容器还是可以提供服务，这种情况请联系云平台管理员排查原因，在恰当的时候重启docker进程或者主机。



