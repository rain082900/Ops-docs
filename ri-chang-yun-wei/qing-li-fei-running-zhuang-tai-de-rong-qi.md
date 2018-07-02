1.查看业务应用运行情况

kubectlget pod -n=namespace          --namespace为对应租户名称

2.找到不正常pod所在主机

3.执行命令强制删除container

4.检查是否恢复正常

