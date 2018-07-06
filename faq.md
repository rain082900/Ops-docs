### FAQ

#### 1.kubernetes dns域名解析错误

版本： K8S/1.5.3，docker/1.13.1

问题描述： 创建用户报错，查看日志发现system与harbor通信时，harbor地址registry.c2cloud.cn解析成了172.16.71.52，实际上该域名绑定的IP是172.17.80.37.

问题分析： 在集群主节点上执行 nslookup registry.c2cloud.cn 发现是K8s的DNS 10.96.0.10解析的该域名，且解析为了以前绑定的IP，因为这台虚拟机以前装过类似的软件，怀疑DNS缓存收到污染。

解决过程：

1.配置宿主据dns

`search svc.cluster.local`

`nameserver 10.96.0.10`

`search novalocal`

`nameserver 114.114.114.114`

2.重启kube-dns

3.重启c2-cce-system

问题解决。

\(kube-dns把域名解析成错误IP\)

---

#### 2.集群监控无法显示节点状态

版本： K8S/1.5.3，docker/1.13.1

问题描述：所有节点运行正常，但是集群监控无法显示节点状态。

解决过程： 经过排查，发现是node节点与master节点时间不同步造成的，用ntp同步时间以后，该节点的监控情况可以正常显示。

---

#### 3.docker修改存储驱动后报错

版本： docker/1.13.1

问题描述： 修改docker存储驱动为devicemapper direct-lvm以后启动docker报错:Error starting daemon: error initializing graphdriver: devmapper: Base Device UUID and Filesystem verification failed: devicemapper: Error running deviceCreate \(ActivateDevice\) dm\_task\_run failed .

问题分析：[ https://github.com/moby/moby/issues/16344            
](https://github.com/moby/moby/issues/16344)

解决过程： 执行 `sh -c 'rm -r /var/lib/docker/*'`  然后重启 `systemctl restart docker`

---

#### 4.进入influxdb容器，命令行执行命令报错

版本： K8S/1.5.3 ， docker/1.13.1，registry.c2cloud.cn/c2cloud/influxdb:1.1.1

问题描述：  进入 容器：kubectl exec -it apps-moniter-4083019374-qhsjl -c influxdb /bin/bash -n=jcptrjb

```
              进入influxdb命令行： influx

              执行命令：show databases

              报错： runtime error: integer divide by zero
```

问题分析： 没有在网上找到产生的原因，现在无法在容器中通过命令行操作influxdb，只能选择替代方案，通过influx的网页客户端执行sql命令。

解决过程： [https://hub.docker.com/\_/influxdb/](https://hub.docker.com/_/influxdb/)

```
             1.在influxdb容器配置里加入环境变量INFLUXDB\_ADMIN\_ENABLED=true

             2.开放8083端口，并映射到nodeport

             3.通过nodeport访问网页客户端，执行sql create database my\_db 创建my\_db数据库。

             4.选择database “my\_db” ,点击“Write Data” 输入

                monit,apikeyId=1,serviceId=1,serviceName=2fg callStatus=1i,requestTime=32i,kongTime=31i,proxyTime=1i

               完成表格创建。
```

---

#### 5.执行命令报错Unable to connect to the server: dial tcp: lookup localhost on 114.114.114.114:53: no such host

版本： K8S/1.5.3 ， docker/1.13.1

问题描述： 完成master节点部署后，执行 kubectl get pod 报错  Unable to connect to the server: dial tcp: lookup localhost on 114.114.114.114:53: no such host

问题分析：localhost无法正常解析。

解决过程： 在 /etc/hosts下配置

127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4

::1 localhost localhost.localdomain localhost6 localhost6.localdomain6

问题解决。

---

#### 6.docker-compose link以后连接不到数据库

版本： docker-compose/1.10.0

问题描述： docker-compose.yaml

```
apigateway:

image: registry.c2cloud.cn/c2cloud/apigateway:0.10.1

container_name: apigateway

restart: always

links:

- kong-redis

- kong-db

depends_on:

- kong-redis

- kong-db

environment:

- KONG_DATABASE=postgres

- KONG_PG_HOST=127.0.0.1

- API_GATEWAY_REDIS_HOST=kong-redis

- API_GATEWAY_REDIS_PORT=6379
```

当 KONG\_PG\_HOST 配置成127.0.0.1或者localhost时连接不到 kong-db。

问题分析： docker-compose只支持域名的形式。

解决过程： 把 127.0.0.1   改为service name  即：- KONG\_PG\_HOST=kong-db

---

#### **7.pod内部不能连接外网**

**版本**：K8S/1.5.3，docker/1.13.1

**问题描述：**

有一台主机上的所有pod都不能连接外网**。**

1.进入容器 `docker exec -it b3da77d4c3e8 /bin/sh`

2.ping [www.baidu.com](http://www.baidu.com) 无响应

**问题分析**：怀疑容器到主机的网络路由没有联通，验证过程如下：

1. ip route show 查看weave的路由为10.32.0.0/12

2. iptables-save\|grep10.32.0.0/12  无记录

**解决过程：**

基本确定是路由设置问题，很可能是该主机加入集群时候没有关闭firewall导致weave路由规则没有加入到iptables，

手动执行命令 `/sbin/iptables -t nat -I POSTROUTING -s 10.32.0.0/12 -j MASQUERADE`

问题解决

---

#### **8.启动docker容器时连接外部oracle数据库不稳定，日志显示connect reset异常**

**版本**：K8S/1.5.3，docker/1.13.1

**问题描述：**测试环境 ，启动uop应用时会用jdbc连接oralce数据库，重启5次只有一次连接成功，其余报错 connect reset**。**

**问题分析**：

1.确认数据库连接信息正确无误

2.确认网络连接正常

3.确认防火墙已经关闭

4.数据库的最大连接数大于当前的连接数

```
SELECT
  'Currently, '
  || (SELECT COUNT(*) FROM V$SESSION)
  || ' out of '
  || VP.VALUE
  || ' connections are used.' AS USAGE_MESSAGE
FROM
  V$PARAMETER VP
WHERE VP.NAME = 'sessions'
```

最后在网上找到一种说法：JDBC连接oracle时会读取系统的安全随机数，默认提供者是/dev/random，如果系统的安全随机数供不应求可能导致java程序阻塞，此时可以换一个全新的安全随机数供应商/dev/urandom。

具体分析 ：

[http://blog.csdn.net/jackie\_xiaonan/article/details/37740359](http://blog.csdn.net/jackie_xiaonan/article/details/37740359)

**解决过程：**

第一种 ：

在tomcat配置文件加入jvm配置

-Djava.security.egd=file:/dev/./urandom

第二种 :

在容器所在机器执行下面命令

rm /dev/random

ln -s /dev/urandom /dev/random

问题解决

---

#### **9.部署flannel作为集群内部网络，无法访问nodeport**

**版本**：K8S/1.5.3，docker/1.13.1，flannel/v0.8.0-amd64

**问题描述：**

使用flannel作为集群叠加网络，无法在浏览器上访问nodeport开放的端口

**问题分析**：

Docker从1.13版本开始调整了默认的防火墙规则，禁用了iptables filter表中FOWARD链，这样会引起Kubernetes集群中跨Node的Pod无法通信

**解决过程：**

在各个Docker节点执行下面的命令

```
iptables -P FORWARD ACCEPT
```

---

#### 10.kong日志信息撑爆磁盘，导致进程无法响应

**版本**：K8S/1.5.3 docker/1.13.1 apigateway:0.10.1-v1

**问题描述**： 压力测试过程中发现api网关不响应，分析发现日志把磁盘撑爆

**问题分析: **在大量访问请求环境中需要调整kong的日志访问级别

**解决过程**：

通过挂载的方式修改/usr/local/share/lua/5.1/kong/templates/nginx\_kong.lua文件中access log 和error log的输出级别

```
location /nginx_status {
        internal;
        access_log off;
        stub_status;
    }
```

```
error_log logs/error.log crit;
```

---

#### 11.集群内部DNS间歇性不能访问

版本: k8s/1.5.3 docker/1.13.1

问题描述： 部署在集群内部的应用之间通过dns访问时间歇性报错：no hosts found

问题分析：通过 kubectl get pod -n=kube-system 查看kube-dns 发现restar了很多次，很可能是分配内存不足

解决过程：通过kubernetes命令行或者dashboard管控端调整kube-dns的内存使用限制

```
kubectl edit deployment kube-dns -n=kube-system -o yaml
```

根据环境调整可以分配的内存

```
resources:
          limits:#最大只能分配170M内存
            memory: 170Mi #请根据实际生产环境调整
          requests:#最少分配100M内存
            cpu: 100m
            memory: 70Mi
```



