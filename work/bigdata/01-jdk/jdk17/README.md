## 安装OpenJDK17

解压软件包

```
tar -zxvf OpenJDK17U-jdk_x64_linux_hotspot_17.0.12_7.tar.gz -C /usr/local/software/
ln -s /usr/local/software/jdk-17.0.12+7 /usr/local/software/jdk17
```

配置环境变量

```
cat >> ~/.bash_profile <<"EOF"
## JAVA_HOME
export JAVA_HOME=/usr/local/software/jdk17
export PATH=$PATH:$JAVA_HOME/bin
EOF
source ~/.bash_profile
```

查看版本

```
java -version
```