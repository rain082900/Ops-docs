### kubernetes常用命令

> kubernetes版本1.5.3,不同版本命令行，略有差异，以下命令行需在集群主节点执行

命令概览

```
kubectl -h
```

查看节点状态

```
kubectl get node -o wide
```

查看namespace列表

```
kubectl get namespace
```

查看namespace下的资源

```
kubectl get pod/deployment/svc/configmap/pvc/secret/ -n={NAMESPACE_NAME} -o wide
```

查看资源详情\(json/yaml\)格式

```
kubectl get pod/deployment/svc/configmap/pvc/secret/ {NAME} -n={NAMESPACE} -o wide -o json/yaml
```

查看资源运行状态

```
kubectl describe pod {POD_NAME} -n={NAMESPACE}
```

查看应用日志

```
kubectl get logs -f {POD_NAME}
```

进入容器

```
kubectl  exec -it {POD_NAME} /bin/bash
```

修改资源运行状态（注意：保存退出资源将重启）

```
kubectl edit pod/deployment/svc/configmap/pvc/secret/ -n={NAMESPACE_NAME} -o json 
```



