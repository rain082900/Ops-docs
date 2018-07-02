                                                                                  部署过程中常见故障，及定位方法

1.node节点时间不能与master节点进行时间同步,提示no server suitable for synchronization found

a.原因 master节点防火墙未关闭

关闭防火墙命令

systemctl stop firewalld.service

systemctl disable firewalld.service

b.master时间服务器上重启ntpd-server服务

systemctl restart ntpd





