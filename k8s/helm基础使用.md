# 安装

1. 软件包下载地址：`https://github.com/helm/helm/releases`
2. 解压：`tar -zxf helm-v3.14.0-linux-amd64.tar.gz`
3. 复制文件：`mv linux-amd64/helm /usr/local/bin/`
4. 判断是否成功：`helm version`
5. 配置命令自动补全
   > source <(helm completion bash)  
   > echo "source <(helm completion bash)" >> ~/.bashrc
6. 添加几个常用仓库
   > helm repo add bitnami https://charts.bitnami.com/bitnami  
   > helm repo add ali https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts

# 常用命令

1. 查看已添加的仓库列表：`helm repo list`
2. 更新仓库本地缓存：`helm repo update`
3. 搜索 charts：`helm search repo <chart名称>`
4. 安装 charts：`helm install <自定义名称> <仓库chart名称>`， 如`helm install mynginx bitnami/nginx`
5. 卸载 charts：`helm uninstall <安装时的自定义名称>`
