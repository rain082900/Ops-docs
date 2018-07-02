1。定期查看主机资源使用情况（内存，磁盘）

查看内存使用情况：free -g

查看磁盘空间: df -h

2。查看docker kubelet进程情况

查看docker日志信息: systemctl status docker -l

查看docker kubelet进程情况:systemctl status  kubelet  -l

3。资源不足的处理方法

执行清理脚本、申请更多资源、主机加入集群

4。定期重启主机

