### 镜像转移

#### 如何把镜像部署到我的生产环境

#### 有外网环境

1.登录公司的镜像仓库 docker login registry.c2cloud.cn   输入用户名/密码

2.上传镜像到公司镜像仓库

```
docker push registry.c2cloud.cn/XXX/imagename:vesion
```

3.在生产环境登录镜像仓库，然后下载镜像

```
docker push registry.c2cloud.cn/XXX/imagename:vesion
```

4.查看本地镜像

```
docker images
```

#### 无外网环境

1.在找一台安装了docker环境的虚拟机

2.登录公司的镜像仓库 docker login registry.c2cloud.cn   输入用户名/密码

3.从公司的镜像仓库下载到本地

```
docker pull registry.c2cloud.cn/xxx/imagename:vesion
```

4.把镜像打成tar包（如果是自己制作的镜像，请忽略1 2 3步）

```
docker save -o test.tar registry.c2cloud.cn/xxx/imagename:vesion
```

5.通过移动设备或者传输工具把tar包传到生产环境服务器

6.解压镜像

```
docker load -i test.tar
```

7.上传镜像到仓库 （假设生产环境镜像仓库地址为10.22.72.6）

```
docker tag registry.c2cloud.cn/xxx/imagename:vesion   10.22.72.6/xxx/imagename:vesion 
docker push 10.22.72.6/xxx/imagename:vesion
```

8.通过应用部署平台或者docker命令运行镜像

```
docker run -d 10.22.72.6/c2cloud/imagename:vesion
```

9查看本地镜像

```
docker images
```

> 注意：上面提到的registry.c2cloud.cn/xxx/imagename:vesion中 “registry.c2cloud.cn”指镜像仓库的访问地址，”xxx“指项目名，可以在镜像仓库的管理页面生成，“imagename”指镜像名称，一般就是项目名称，“version”指镜像版本，一般就是项目的版本号。需要根据本地环境替换。
>
> 镜像仓库部署请参见云平台部署文档



