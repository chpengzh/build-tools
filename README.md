# 容器化构建工具链

[docker hub](https://cloud.docker.com/repository/docker/chpengzh/build-tools)

经常要在离线隔离环境下安装JAVA研发机器，所以索性就将研发机器的环境容器化了

## 工具列表

基于 Debain:9 操作系统

目前预装了如下研发工具与服务

- [x] openjdk:8
- [x] maven:3.6.0
- [x] gradle:5.3.1
- [x] docker-ce:18.09.4

## 如何使用

### 1. 构建工具链

> 构建基础镜像

```
> cd simple && docker build . -t build-tools
```

> 构建桌面镜像

```
> cd desktop && docker build . -t build-tools-desktop
```

>  在`~/.bashrc`(或`~/.zshrc`)中添加alias


```
alias build-tools='docker run -it \
    --privileged \
    --rm  \
    -v $(pwd):/root/share \
    build-tools'
alias build-tools-detached='docker run  -dt \
    --privileged \
    --name ${PWD##*/}_$(date "+%Y%m%d-%H:%M:%S") \
    -v $(pwd):/root/share \
    build-tools'
alias build-tools-desktop='docker run -dt \
    --privileged \
    --name debain-desktop \
    -e DISPLAY=unix$DISPLAY \
    -p 5901:5901 \
    -v $(pwd):/root/share \
    build-tools-desktop'
```

### 2. 使用工具链

> 可以在当前命令行环境下启动一个独立的构建容器进程

```
> build-tools
```

> 也可以启动一个独立运行的容器来处理构建生命周期

```
> build-tools-detached
> docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
029ad7c0c97a        build-tools         "/bin/sh -c 'bash -e…"   6 seconds ago       Up 6 seconds                            spring-framework_20190406-154750
```

> 如果是在独立容器中编译，可以使用exec的方式进入容器编译

```
> docker exec -it spring-framework_20190406-154750 bash
#> pwd
/root/share
#> 
#> ls -alt
root@029ad7c0c97a:~/share# ls
CODE_OF_CONDUCT.adoc  build	    gradle	       gradlew.bat	       settings.gradle	spring-beans		spring-context-support	spring-expression     spring-jcl   spring-messaging  spring-test  spring-webflux    src
CONTRIBUTING.md       build.gradle  gradle.properties  import-into-eclipse.md  spring-aop	spring-context		spring-core		spring-framework-bom  spring-jdbc  spring-orm	     spring-tx	  spring-webmvc
README.md	      buildSrc	    gradlew	       import-into-idea.md     spring-aspects	spring-context-indexer	spring-core-coroutines	spring-instrument     spring-jms   spring-oxm	     spring-web   spring-websocket
#>
#> gradle build
#> .........
```
