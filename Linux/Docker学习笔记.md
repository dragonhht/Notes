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

## 1, 创建容器

