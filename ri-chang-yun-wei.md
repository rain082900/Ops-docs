### 日常运维

#### 如何制作镜像

> 这里以制作基于tomcat的应用镜像为例

##### 制作镜像前需要准备

docker环境

tomcat基础镜像（registry.c2cloud.cn/library/tomcat8:latest）

应用war包

##### 准备Dockerfile

```
#编辑Dockerfile文件
vi Dockerfile
```

##### Dockerfile模板：

```
FROM registry.c2cloud.cn/library/tomcat8:latest
MAINTAINER Jungle <hongjiang.liu@chinacreator.com>
# 在当前环境创建target文件夹，把war包放进去
COPY target/*.war /opt/tomcat/webapps/ROOT.war
# Launch Tomcat
CMD ["/opt/tomcat/bin/catalina.sh", "run"]
```

```
#上传war包到target目录
#在Dockerfile所在目录执行命令
docker build -t IMAGENAME:VERSION .
```

#### 

#### 没有外网的环境下如何把我的镜像部署到生产环境

1.在公司环境找一个安装了docker环境的虚拟机

2.登录公司的镜像仓库 docker login registry.c2cloud.cn   输入用户名/密码

3.从公司的镜像仓库下载到本地

```
docker pull registry.c2cloud.cn/c2cloud/imagename:vesion
```

4.把镜像打成tar包

```
docker save -o test.tar registry.c2cloud.cn/c2cloud/imagename:vesion
```

5.通过移动设备或者传输工具把tar包传到生产环境服务器

6.解压镜像

```
docker load -i test.tar
```

7.上传镜像到仓库 （假设生产环境镜像仓库地址为10.22.72.6）

```
docker tag registry.c2cloud.cn/c2cloud/imagename:vesion   10.22.72.6/c2cloud/imagename:vesion 
docker push 10.22.72.6/devp/imagename:vesion
```

8.通过应用部署平台或者docker命令运行镜像

```
docker run -d 10.22.72.6/c2cloud/imagename:vesion
```



