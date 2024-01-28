# 安装

## 1. 说明

| 主机            | 说明        |
| --------------- | ----------- |
| 192.168.133.130 | master 节点 |
| 192.168.133.131 | node1 节点  |
| 192.168.133.132 | node2 节点  |

系统：CentOS 8

## 2. 环境初始化

### 2.1 主机名解析

> 编辑三台服务器的/etc/hosts 文件，添加以下内容  
> 192.168.133.130 master  
> 192.168.133.131 node1  
> 192.168.133.132 node2

### 2.2 时间同步

同步各节点的时间让其保持一致，可执行以下语句

> systemctl start chronyd  
> systemctl enable chronyd  
> date

### 2.3 firewalld 服务

> systemctl stop firewalld  
> systemctl disable firewalld

如果使用的是`iptables`，则需要禁用`iptables`服务

### 2.4 禁用 selinux

修改`/etc/selinux/config`文件，修改 SELINUX 的值为 disable

> SELINUX=disabled

### 2.5 禁用 swap 分区

编辑`/etc/fstab`，注释掉 swap 分区一行

### 2.6 修改 linux 的内核参数

1. 修改`/etc/sysctl.d/kubernetes.conf`文件，添加如下配置

> net.bridge.bridge-nf-call-ip6tables = 1  
> net.bridge.bridge-nf-call-iptables = 1  
> net.ipv4.ip_forward = 1

2. 加载配置

> sysctl -p

3. 加载网桥过滤模块

> modprobe br_netfilter

4. 查看网桥过滤模块是否加载成功

> lsmod | grep br_netfilter

### 2.7 配置 ipvs 功能

1. 安装 ipset 和 ipvsadm

> yum install ipset ipvsadm -y

2. 添加需要加载的模块写入脚本文件

> cat <<EOF> /etc/sysconfig/modules/ipvs.modules  
> #!/bin/bash  
> modprobe -- ip_vs  
> modprobe -- ip_vs_rr  
> modprobe -- ip_vs_wrr  
> modprobe -- ip_vs_sh  
> modprobe -- nf_conntrack_ipv4  
> EOF

3. 为脚本添加执行权限

> chmod +x /etc/sysconfig/modules/ipvs.modules

4. 执行脚本文件

> /bin/bash /etc/sysconfig/modules/ipvs.modules

5. 查看对应的模块是否加载成功

> lsmod | grep -e ip_vs -e nf_conntrack_ipv4

注：`以上系统配置完成后需要重启系统使其生效`

### 2.8 安装 Containerd

该部分内容可查看[Containerd 学习笔记](../Linux/Containerd%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.md)

1. 安装完成后生成`config.toml`文件

> containerd config default > /etc/containerd/config.toml

2. 将 sandbox_image 下载地址改为阿里云地址

> [plugins."io.containerd.grpc.v1.cri"]  
> ...  
>  sandbox_image = "registry.aliyuncs.com/google_containers/pause:3.9"

## 3. 安装 k8s

### 3.1 k8s 配置阿里云 yum 源

> cat <<EOF > /etc/yum.repos.d/kubernetes.repo  
> [kubernetes]  
> name = Kubernetes  
> baseurl = https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64  
> enabled = 1  
> gpgcheck = 0  
> repo_gpgcheck = 0  
> gpgkey = https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg  
> EOF

### 3.2 yum 安装 kubeadm、kubelet、kubectl

#### 3.2.1 删除历史版本

> yum -y remove kubelet kubeadm kubectl

#### 3.2.2 安装 kubeadm、kubelet、kubectl(此处固定版本：1.28，可根据情况修改)

> yum install -y kubelet-1.28.0 kubeadm-1.28.0 kubectl-1.28.0 --disableexcludes=kubernetes  
> systemctl enable kubelet

#### 3.2.3 集群镜像准备

因为网路问题，如果无法正常拉去进行则可进行以下处理

- 查看当前版本需要的镜像

> kubeadm config images list

- 根据输出后需要的镜像列表，修改镜像库拉去镜像,其中 images 的内容根据上一步输出的结果进行修改

> images=(  
>  kube-apiserver:v1.28.2  
> kube-controller-manager:v1.28.2  
> kube-scheduler:v1.28.2  
> kube-proxy:v1.28.2  
> pause:3.9  
> etcd:3.5.9-0  
> coredns:v1.10.1  
> )
>
> for imageName in ${images[@]};do  
>	ctr images pull registry.cn-hangzhou.aliyuncs.com/google_containers/$imageName  
> ctr images tag registry.cn-hangzhou.aliyuncs.com/google_containers/$imageName k8s.gcr.io/$imageName  
> ctr images rm registry.cn-hangzhou.aliyuncs.com/google_containers/$imageName  
> done

#### 3.2.4 集群初始化

以下操作只在 master 节点执行

1. 创建集群

> kubeadm init \  
> --apiserver-advertise-address=192.168.133.130 \  
> --image-repository registry.aliyuncs.com/google_containers \  
> --kubernetes-version v1.28.0 \  
> --service-cidr=10.96.0.0/12 \  
> --pod-network-cidr=10.244.0.0/16 \

2. 根据初始化后控制台输出的内容提示进行处理及其他 node 节点的参加

3. 在 master 上查看节点信息

> kubectl get nodes

#### 3.2.5 部署 CNI 网络

1. 下载 cni 插件

> wget https://github.com/containernetworking/plugins/releases/download/v1.3.0/cni-plugins-linux-amd64-v1.3.0.tgz  
> mkdir-pv /opt/cni/bin  
> tar zxvf cni-plugins-linux-amd64-v1.3.0.tgz -C /opt/cni/bin/

2. master 安装 flannel

> kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
