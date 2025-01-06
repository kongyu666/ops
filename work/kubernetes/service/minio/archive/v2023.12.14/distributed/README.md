# 创建MinIO

修改配置

> values.yaml是修改后的配置，可以根据环境做出适当修改，例如修改存储类global.storageClass

```
cat values.yaml
```

创建服务

```shell
helm install minio -n kongyu -f values.yaml minio-12.10.10.tgz
```

查看服务

```
kubectl get -n kongyu pod,svc,pvc -l app.kubernetes.io/instance=minio
kubectl logs -f -n kongyu minio-0
```

使用服务

```
URL: http://192.168.1.19:48887/
Username: admin
Password: Admin@123
```

删除服务以及数据

```
helm uninstall -n kongyu minio
kubectl delete pvc -n kongyu data-minio-{0..3}
```
