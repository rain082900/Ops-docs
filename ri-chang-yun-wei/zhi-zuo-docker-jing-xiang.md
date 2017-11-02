### 制作docker镜像

> 这里以制作基于tomcat的应用镜像为例

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



