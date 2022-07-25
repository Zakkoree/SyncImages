# Dockerfile
尽量使用alpine操作系统作为base image
### 1. 上传jar包和Dockerfile文件至安装docker的linux目录
Dockerfile文件内容

```docker
# 基础镜像，使用alpine操作系统，openjkd使用8u201
FROM openjdk:8-jdk-alpine
# 镜像制作人信息
LABEL maintainer="xxx"<xxxxx@xx.com>
# 版本号
LABEL version="1.0"
# 描述说明
LABEL description="This is description"
# 切换root用户
USER root
#系统编码
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
# 项目环境变量
ENV SPRING_PROFILES_ACTIVE=prod
# 执行工作目录
WORKDIR application
# 声明一个挂载点
# 容器内此路径会对应宿主机的某个文件夹
# 该目录必须提前创建
VOLUME /data
# 应用构建成功后的jar文件被复制到镜像内，名字也改成了app.jar
ADD *.jar app.jar
# 启动容器时的进程
ENTRYPOINT ["sh","-c","java -jar $JAVA_OPTS app.jar"]
```
### 2. 使用docker build命令创建镜像
```docker build -t app:1.0 .```

### 3. 保存镜像到本地
docker save -o app.tar app

### 4. 运行镜像
docker run -d -p 8080:8080 --name app app:1.0
