#### **1.查看业务应用运行情况**

`kubectl get pod -n=namespace          --namespace为对应租户名称`

#### **2.找到不正常pod所在主机**

`kubectl get pods -o wide -n=namespace          --namespace为对应租户名称`

![](/assets/6.png)

#### **3.执行命令强制删除container**

a.登录node节点,查找应用表示命令:

`docker ps |grep apps-integration`![](/assets/21.png)b.删除不正常docker id命令:

`docker rm -f docker_id`

#### **4.检查是否恢复正常**

Running状态为正常![](/assets/7.png)

