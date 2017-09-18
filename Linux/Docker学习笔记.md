# 1, 安装

## 1， Ubuntu环境下

-   注意

    1.  内核不低于3.10

    2.  64位平台

- 使用curl安装

    1.  检查是否安装curl `which curl`

    2.  如果curl没有安装的话，更新apt源之后，安装curl包 `sudo apt-get update $ sudo apt-get install curl`

    3.  获得最新的docker安装包 `curl -sSL https://get.docker.com/ | sh `

    4.  将用户加入到docker用户组 `sudo usermod -aG docker USER_NAME`

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

    2. 基于本地模板导入 ， 例如`docker import - ubuntu:14.04`

-   存出镜像，导出镜像到本地文件，如`docker save -o ubuntu_14.04.tar ubuntu:14.04`

-   载入镜像,将导出的tar文件在导入到本地镜像库， 如`docker load --input ubuntu_14.tar`

# 3, 操作Docker

-   查看本机的所有容器 `docker ps -a`, 查看运行的容器 `docker ps`

-   创建容器 `docker create -it NAME[:TAG]`, 或新建并启动容器命令 `docker run -it NAME[:TAG]`

-   启动容器 `docker start 容器名或ID`

-   终止容器 `docker stop 容器名或ID`

-   进入容器

    1.  使用`attach`命令：`docker attach 容器名或ID`

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

# 8, 数据库的使用

## 1， MYSQL

-   启动实例：`docker run -e MYSQL_ROOT_PASSWORD=密码 -d mysql[:TAG]`

-   使用`docker exec`调用内部系统的bash shell, 如：`docker exec -it 容器名或ID bash`,进入容器后，可进入MySQL客户端

## 2， redis

-   启动redis容器：`docker run --name redis -d redis`

-   进入容器内部` docker exec -it 容器名或ID bash`