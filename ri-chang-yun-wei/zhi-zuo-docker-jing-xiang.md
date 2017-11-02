### 制作docker镜像

docker镜像作为一种标准交付在实际部署运维过程中非常的重要，很多镜像可以在公司镜像仓库或者docker官方镜像仓库下载，有时候也需要我们制作自己的镜像，最常见的场景就是我的项目已经通过maven打成了war包，如何把我的war包变成docker镜像，部署到生产环境呢？  
下面以制作基于tomcat启动的java工程为例，一步步解析如何制作自己的镜像。

##### 制作镜像前需要准备

docker环境

tomcat基础镜像（registry.c2cloud.cn/library/tomcat8:latest 可以在公司镜像仓库下载）

应用war包——需要部署的应用，用maven build打成的war包

##### 准备Dockerfile

```
#编辑Dockerfile文件
vi Dockerfile
--------------------------------------------------
FROM registry.c2cloud.cn/library/tomcat8:latest
MAINTAINER Jungle <hongjiang.liu@chinacreator.com>
# 在当前环境创建target文件夹，把war包放进去
COPY target/*.war /opt/tomcat/webapps/ROOT.war
# Launch Tomcat
CMD ["/opt/tomcat/bin/catalina.sh", "run"]
```

##### 打成镜像

```
#上传war包到target目录
#在Dockerfile所在目录执行命令
docker build -t IMAGENAME:VERSION .
```

执行docker build命令以后，等待运行完成，将会在本机上生成由你命名为“IMAGENAME:VERSION”的 docker镜像，可以通过`docker images` 命令查看。

> Dockerfile 语法请参见[常用命令](/chang-yong-ming-ling.md)



