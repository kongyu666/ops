## SpringBoot应用

使用Dockerfile构建SpringBoot应用



**修改Dockerfile**

- 修改Jar包路径：ARG JAR_FILE
- 设置应用运行的参数：JAVA_OPTS
- 设置应用的端口：EXPOSE

**构建镜像**

```shell
docker build -t registry.lingo.local/service/app:springboot2-demo-openjdk8-debian .
```

**测试镜像**

```shell
docker run --rm -p 8888:8888 registry.lingo.local/service/app:springboot2-demo-openjdk8-debian
```

**推送镜像**

```shell
docker push registry.lingo.local/service/app:springboot2-demo-openjdk8-debian
```

