### 常用命令

#### docker常用命令

> docker版本V1.13.1，不同版本docker命令略有差异。

docker命令概览

```
docker --help
```

查看当前版本

```
docker version
```

查看本地镜像列表

```
docker images
```

查看运行中的容器列表

```
docker ps
```

查看容器日志

```
docker logs -f {containerID}
```

查看容器实时资源使用情况

```
docker stats {containerID}
```

删除容器

```
docker rm -i {containerID}
```

启动/重启/停止docker服务（centos7）

```
systemctl start/restart/stop docker
```

批量删除镜像

`docker images|grep none|awk ‘{print $3}’|xargs docker rmi`

创建容器

`docker run --name my_container_name -p 3306:3306 -v /data:/var/mysql/data -e MYSQL_ROOT_PASSWORD=my_password -d mysql:5.5.1`

切换到容器环境

`docker exec -it mycontainer_name /bin/bash`

查看容器信息

`docker inspect --format {{'json .Mounts'}} APP2`

#### 

#### Dockerfile基本语法

FROM 便是上下文环境 即应用的运行环境\(告诉docker使用哪个镜像作为基础\)

MAINTAINER 维护者信息

RUN 表示要执行的命令

ADD 复制本地文件到镜像

EXPOSE 对外开发的端口

CMD 容器启动后运行的程序

> docker 命令参考[ https://docs.docker.com/engine/reference/commandline/cli/](https://docs.docker.com/engine/reference/commandline/cli/)
>
> dockerfile 语法参考 [https://docs.docker.com/engine/reference/builder/\#run](https://docs.docker.com/engine/reference/builder/#run)



