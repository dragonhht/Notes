# YUM 方式安装

1. 获取 yum 软件源

> wget -O /etc/yum.repos.d/docker-ce.repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

2. 查询软件版本

> yum list | grep containerd

3. 安装 containerd

> yum -y install containerd.io

4. 查询是否安装

> rpm -qa | grep containerd

5. 设置开机自启并启动服务

> systemctl enable containerd  
> systemctl start containerd

6. 查看服务启动状态

> systemctl enable containerd

7. 验证是否可用

> ctr version

# 命令

各命令可通过`--help`查询详细信息

1. 容器镜像管理命令`ctr images`

2. 静态容器: `ctr container`或`ctr c`

3. 动态容器(任务): `ctr task`或`ctr t`

# 问题

## Containerd 提示 runc: symbol lookup error: runc: undefined symbol: seccomp_notify_respond

> 这个是说缺少依赖包 libseccomp，需要注意的是 centos7 中 yum 下载的版本是 2.3 的，版本不满足我们最新 containerd 的需求，需要下载 2.4 以上的

1. yum 更新

> yum install libseccomp -y
