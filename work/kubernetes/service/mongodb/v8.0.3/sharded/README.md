# MongoDB

MongoDB 是一种基于文档的 NoSQL 数据库，以高性能、易扩展和灵活的文档存储而著称。它由 C++ 语言编写，于 2009 年首次发布。与传统的关系型数据库（如 MySQL、PostgreSQL）不同，MongoDB 采用非结构化的数据存储方式，不使用表和行，而是通过集合（Collection）和文档（Document）来组织数据。

**查看版本**

```
helm search repo bitnami/mongodb-sharded -l
```

**下载chart**

```
helm pull bitnami/mongodb-sharded --version 9.0.3
```

**修改配置**

values.yaml是修改后的配置，可以根据环境做出适当修改

- 存储类：defaultStorageClass（不填为默认）
- 认证配置：auth.rootUser auth.rootPassword
- 镜像地址：image.registry
- 其他配置：...

```
cat values.yaml
```

**创建标签，运行在标签节点上**

```
kubectl label nodes server02.lingo.local kubernetes.service/mongodb="true"
kubectl label nodes server03.lingo.local kubernetes.service/mongodb="true"
```

**创建服务**

```
helm install mongodb -n kongyu -f values.yaml mongodb-sharded-9.0.3.tgz
```

**查看服务**

```
kubectl get -n kongyu pod,svc,pvc -l app.kubernetes.io/instance=mongodb
kubectl logs -f -n kongyu deploy/mongodb-mongos
```

**使用服务**

创建客户端容器

```
kubectl run mongodb-client --rm --tty -i --restart='Never' --image  registry.lingo.local/bitnami/mongodb-sharded:8.0.3 --namespace kongyu --command -- bash
```

内部网络访问

```
mongosh admin --host "mongodb.kongyu:27017" --authenticationDatabase admin -u root -p Admin@123 --eval "sh.status()"
```

集群网络访问

> 使用集群+NodePort访问

```
mongosh admin --host "192.168.1.10:14209" --authenticationDatabase admin -u root -p Admin@123 --eval "sh.status()"
```

**服务扩缩容**

> 将**mongodb-shard**服务扩展至5个副本

```
helm upgrade mongodb -n kongyu -f values.yaml --set shards=5 mongodb-sharded-9.0.3.tgz
kubectl get -n kongyu pod,svc,pvc -l app.kubernetes.io/instance=mongodb
```

**删除服务以及数据**

```
helm uninstall -n kongyu mongodb
kubectl delete -n kongyu pvc -l app.kubernetes.io/instance=mongodb
```

