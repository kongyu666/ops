# Mariadb Galera

MariaDB 是 MySQL 的一个分支，具有高性能、开源、兼容性强等特点。它提供关系型数据库管理功能，支持多种存储引擎和复杂查询操作，适用于各种应用场景。

MariaDB Galera 是 MariaDB 的高可用性解决方案，基于 Galera Cluster 技术，支持多主同步复制。它允许所有节点都能进行读写操作，提供数据一致性和自动故障恢复，适合对高可用性和扩展性要求较高的数据库环境。

https://mariadb.org/

https://galeracluster.com/

**查看版本**

```
helm search repo bitnami/mariadb-galera -l
```

**下载chart**

```
helm pull bitnami/mariadb-galera --version 14.0.10
```

**修改配置**

根据环境做出相应的修改

```
cat values.yaml
```

**创建标签，运行在标签节点上**

```
kubectl label nodes server02.lingo.local kubernetes.service/mariadb-galera="true"
kubectl label nodes server03.lingo.local kubernetes.service/mariadb-galera="true"
```

**创建服务**

> 可以动态伸缩节点

```shell
helm install mariadb-galera -n kongyu -f values.yaml mariadb-galera-14.0.10.tgz
```

**查看服务**

```
kubectl get -n kongyu pod,svc,pvc -l app.kubernetes.io/instance=mariadb-galera
kubectl logs -f -n kongyu -l app.kubernetes.io/instance=mariadb-galera
```

**使用服务**

创建客户端容器

```
kubectl run mariadb-galera-client --rm --tty -i --restart='Never' --image  registry.lingo.local/service/mariadb-galera:11.4.3 --namespace kongyu --command -- bash
```

内部网络访问-headless

```
mariadb -hmariadb-galera-0.mariadb-galera-headless.kongyu -uroot -pAdmin@123 -e "select * from mysql.wsrep_cluster_members"
```

内部网络访问

```
mariadb -hmariadb-galera.kongyu -uroot -pAdmin@123 -e "select * from mysql.wsrep_cluster_members"
```

集群网络访问

> 使用集群+NodePort访问

```
mariadb -h192.168.1.10 -P15842 -uroot -pAdmin@123 -e "select * from mysql.wsrep_cluster_members"
```

**删除服务以及数据**

```
helm uninstall -n kongyu mariadb-galera
kubectl delete -n kongyu pvc -l app.kubernetes.io/instance=mariadb-galera
```
