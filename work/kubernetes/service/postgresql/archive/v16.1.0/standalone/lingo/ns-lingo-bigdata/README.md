# 安装postgresql

修改配置

> values.yaml是修改后的配置，可以根据环境做出适当修改，例如修改存储类global.storageClass

```
cat values.yaml
```

创建服务

```shell
helm install postgresql -n lingo-bigdata -f values.yaml postgresql-13.2.24.tgz
```

查看服务

```
kubectl get -n lingo-bigdata pod,svc,pvc -l app.kubernetes.io/instance=postgresql
kubectl logs -f -n lingo-bigdata postgresql-0
```

使用服务

```
kubectl run postgresql-client --rm --tty -i --restart='Never' --image  registry.lingo.local/service/postgresql:16.1.0 --namespace lingo-bigdata --env="PGPASSWORD=Admin@123" --command -- psql --host postgresql -U postgres -d postgres -p 5432 -c "\l"
## 查看所有配置
SELECT name, setting FROM pg_settings
```

删除服务以及数据

```
helm uninstall -n lingo-bigdata postgresql
kubectl delete -n lingo-bigdata pvc data-postgresql-0
```

