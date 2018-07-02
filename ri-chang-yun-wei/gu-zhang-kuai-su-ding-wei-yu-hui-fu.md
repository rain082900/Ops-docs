```
                                                                              部署过程中常见故障，及定位方法
```

1.node节点时间不能与master节点进行时间同步,提示no server suitable for synchronization found

a.原因 master节点防火墙未关闭

关闭防火墙命令

systemctl stop firewalld.service

systemctl disable firewalld.service

b.master时间服务器上重启ntpd-server服务

systemctl restart ntpd

2.harbor安装提示![](/assets/12.png)需要在解压后的harbor目录下执行下install.sh

然后执行

docker-compose down

docker-compose up -d

3.docker登录harbor，提示https错误

修改docker配置文件，忽略https警告

vi /usr/lib/systemd/system/docker.service; 新增参数

ExecStart=/usr/bin/docker  --insecure-registry 0.0.0.0/0

