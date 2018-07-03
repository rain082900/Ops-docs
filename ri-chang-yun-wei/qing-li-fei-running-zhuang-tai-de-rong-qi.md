#### **1.查看业务应用运行情况**

kubectl get pod -n=namespace          --namespace为对应租户名称

#### **2.找到不正常pod所在主机**

kubectl get pods -o wide -n=namespace          --namespace为对应租户名称

![](/assets/6.png)

#### **3.执行命令强制删除container**

kubectl delete pod podname -n=namespace       ---删除namespace租户下名称是podname的pod

#### **4.检查是否恢复正常**

Running状态为正常![](/assets/7.png)

