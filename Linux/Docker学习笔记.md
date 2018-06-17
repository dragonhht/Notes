# 1, 安装

## 1， Ubuntu环境下

-   注意

    1.  内核不低于3.10

    2.  64位平台

-   安装

    1.  更新软件源并安装docker：`sudo apt-get update & sudo apt-get install docker-ce`

    2.  启动docker服务：`sudo service docker start`

    3.  将用户加入到docker用户组 `sudo usermod -aG docker USER_NAME`

# 2, 相关命令

-   获取镜像 `docker pull NAME[:TAG]` , 如获取Ubuntu 14.04版镜像 `docker pull ubuntu:14.04`, TAG不显示说明自动为最新版

-   运行容器 `docker run -it 容器名或ID`

-   查看镜像信息

    1.  列出本机已有镜像 `docker images`

    2.  查看镜像详细信息 `docker inspect NAME[:TAG]`

    3.  查看镜像历史 `docker history NAME[:TAG]`

-   为镜像添加标签 `docker tag NAME[:TAG] NEWNAME[:TAG]`

-   搜寻镜像 `docker search TERM`

-   删除镜像 `docker rmi IMAGE` , IMAGE 可为便签或ID， 使用 `-f`参数可强制删除

-   创建镜像

    1.  基于已有镜像的容器创建 `docker commit -c "描述" -a "作者信息" 使用ID NEWNAME:TAG`

    2.  基于本地模板导入 ， 例如`docker import - ubuntu:14.04`

-   存出镜像，导出镜像到本地文件，如`docker save -o ubuntu_14.04.tar ubuntu:14.04`

-   载入镜像,将导出的tar文件在导入到本地镜像库， 如`docker load --input ubuntu_14.tar`

# 3, 操作Docker

-   查看本机的所有容器 `docker ps -a`, 查看运行的容器 `docker ps`

-   创建容器 `docker create -it NAME[:TAG]`, 或新建并启动容器命令 `docker run -it NAME[:TAG]`

-   启动容器 `docker start 容器名或ID`

-   终止容器 `docker stop 容器名或ID`

-   进入容器

    1.  使用`attach`命令：`docker attach 容器名或ID`, 退出时使用快捷键`Ctrl + P + Q`退出容器不停止，使用`exit`退出时，容器将关闭

    2.  使用`exec`命令(推荐)： `docker exec -it 容器名或ID /bin/bash`

    3.  使用`nsenter`工具

-   删除容器 `docker rm 容器名或ID`

-   导出容器， 如`docker export -o ubuntu_for_run.tar c51d4f7ef554`

-   导入容器， 如`docker import ubuntu_for_run.tar  test/ubuntu:1.0`

# 4， Docker仓库


# 5, Docker数据管理


# 6， 端口映射与容器互联

-   使用在`docker run` 时使用`-p`参数，格式为：`-p IP:HostPort:ContainerPort|IP:ContrinerPort|Host:ContainerPort`, 例如映射至本机5000端口`docker run -it -p 5000:5000 ubuntu`，或指定IP：`docker run -it -p 127.0.0.1：5000:5000 ubuntu`

-   使用`IP::ContainerPort`,绑定任意端口到容器的某一端口，本地主机会自动分配一个端口，如：`docker run -it -p 127.0.0.1::5000 ubuntu`，每次开启可能发生改变

-   查看端口映射配置，如`docker port 872c825d8cf6 5000`

-   容器互联，使用`--link`连接容器，如：`docker run -it --link 容器名:别名 镜像名或ID`,使用是在容器内通过ip使用,ip可通过`/etc/hosts`查看

# 7, 使用Dockerfile创建镜像

## 1, 使用dockerfile创建

-   创建一个文件夹 ，`mkdir test_docker`

-   进人文件夹，创建Dockerfile和run.sh文件，`touch Dockerfile run.sh`

-   编写dockerfile文件

-   创建镜像：`docker build -t 镜像名 dockerfile路径（在哪目录下即可）`


### 1, build命令

> 用于构建image

-   格式

    > docker build [options] path | url | -  
    > -f            强制使用自定义的构建文件  
    > --rm=true     设置构建完成后是否删除临时文件  
    > -t            指定镜像tag

> build命令中path、url和-（如果使用URL则URL参数必须指向一个git地址）是必须项，至少需要输入一个， docker在构建一个image时，需要读取一个配置文件。这个配置文件就是Dockerfile。这个文件的默认文件名为Dockerfile，但也可以是其他名，不一定是Dockerfile。用户可以通过path|url|-来指定Dockerfile

## 2、DockerFile文件

> Dockerfile语法，就是命令+参数+参数...

### 1、Dockerfile的内置命令

| 内置命令       | 作用                                |
| ---------- | --------------------------------- |
| FROM       | 指明基础镜像名称。必填                       |
| MAINTAINER | 可用于提供作者、版本及其他备注信息。可选              |
| RUN        | 用于执行后面的命令，当RUN执行完毕后，将产生一个新的文件层。可选 |
| CMD        | 指定镜像启动时默认执行命令。可选                  |
| LABEL      | 用于在镜像中添加元数据。例如版本号、构建日期等。可选        |
| EXPOSE     | 用于指定需要暴露的网络端口号可选                  |
| ENV        | 用于在镜像中添加环境变量。可选                   |
| ADD        | 向镜像添加新的文件或者新目录。可选                 |
| COPY       | 从主机向镜像复制文件。可选                     |
| ENTRYPOINT | 在镜像中设定默认执行的二进制程序。可选               |
| VOLUME     | 向镜像中挂在一个卷组。可选                     |
| USER       | 在镜像构建过程中，生成或者切换到另一用户。可选           |
| WORKDIR    | 设置镜像后续操作的默认工作目录。可选                |
| ONBUILD    | 配置构建触发指令集。可选                      |

### 2、示例

```dockerfile
# 设置基础镜像
FROM ubuntu

# 备注信息
MAINTAINER Example dragonhht

# 更新软件源
RUN apt-get update

# 安装vim
RUN apt-get install -y vim

# 暴露端口
EXPOSE 22

RUN echo "完成"
```



# 8, 数据库的使用

## 1， MYSQL

-   启动实例：`docker run -e MYSQL_ROOT_PASSWORD=密码 -d mysql[:TAG]`

-   使用`docker exec`调用内部系统的bash shell, 如：`docker exec -it 容器名或ID bash`,进入容器后，可进入MySQL客户端

## 2， redis

-   启动redis容器：`docker run --name redis -d redis`

-   进入容器内部` docker exec -it 容器名或ID bash`

# 9, WEB服务与应用

