# 1、 构建

-   build命令

    -   格式(记录常用参数)

    > docker build [options] path | url | -  
    > -f            强制使用自定义的构建文件  
    > --rm=true     设置构建完成后是否删除临时文件  
    > -t            指定镜像tag

    -   说明

    > build命令中path、url和-（如果使用URL则URL参数必须指向一个git地址）是必须项，至少需要输入一个， docker在构建一个image时，需要读取一个配置文件。这个配置文件就是Dockerfile。这个文件的默认文件名为Dockerfile，但也可以是其他名，不一定是Dockerfile。用户可以通过path|url|-来指定Dockerfile

# 2、 创建容器

- create命令

  > 用于创建一个docker容器，与`run`命令不同，创建后容器不会立即运行

  - 格式

    > docker create [options] NAME[:TAG]

# 3、使用操作

-   cp命令

    > 容器和主机之间传输文件的命令

    -   格式

    > docker cp [options] 容器名或ID:path 主机文件地址 | - 

# 4、 查看信息

- info命令

> 用于显示docker摘要信息，格式如`docker info [options]`

- inspect命令

> docker inspect [options] 容器|镜像

- stats命令

> 用于查看容器资源情况，docker stats 容器

- top命令

> 查看容器CPU、内存和网络吞吐数据，docker top 容器