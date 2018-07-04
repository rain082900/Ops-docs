### 部署过程中常见故障，及定位方法

##### **1.node节点时间不能与master节点进行时间同步,提示no server suitable for synchronization found**

a.原因 master节点防火墙未关闭

master节点关闭防火墙命令

`systemctl stop firewalld.service`

`systemctl disable firewalld.service`

b.master时间服务器上重启ntpd-server服务

`systemctl restart ntpd`

node节点上执行同步命令:

```
/usr/sbin/ntpdate master-ip; /sbin/hwclock -w
```

**2.harbor安装提示**![](/assets/12.png)需要在解压后的harbor目录下执行下install.sh

然后执行

`docker-compose down`

`docker-compose up -d`

##### **3.docker登录harbor，提示https错误**

修改docker配置文件，忽略https警告

`vi /usr/lib/systemd/system/docker.service;`新增参数

`ExecStart=/usr/bin/docker  --insecure-registry 0.0.0.0/0`

##### **4.执行脚本文件提示：/bin/bash^M: bad interpreter kube.tar**

`vi 文件`

`输入`

`:set ff?`

如果提示![](/assets/13.png)

再次输入 :`set ff=unix`回车\(Enter\) 即可正常运行脚本文件.

##### **5.部署专有云平台后，需要替换相关应用里环境变量apigateway.admin的域名地址为映射的ip地址和端口.**

##### **6.oracle数据库连接失败，应用不能正常运行**

a.确保数据库连接信息配置正确

b.检测oracle数据库版本，如果是12c的 尝试用下面格式

aip\_db\_url=jdbc:oracle:thin:@10.0.203.17:1521:orclpdb  ------------oracle12c以下

aip\_db\_url=jdbc:oracle:thin:@192.168.1.24:1521/orclpdb ----------oracle 12c

c.检测应用tomcat有无配置密码随机参数![](/assets/17.jpg)

##### **7.浏览器访问应用出现报错信息**

chrome浏览器: 按键F12 ,然后重新刷新，查看出现的100-599错误,简单判断反馈问题

##### **8.应用无法启动 提示**![](/assets/14.png)

license过期 参考替换应用license 解决.

### 运行过程中常见故障，及定位方法

##### **1.应用日志显示**

![](/assets/29.png)

![](/assets/29-2.png)

![](/assets/29-1.png)

这几种情况都是应用时间服务不同步引起的.

解决恢复： 同步node节点与master节点的时间，保持一致.

