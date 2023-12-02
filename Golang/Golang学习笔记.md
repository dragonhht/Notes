# Golang学习

## 1、环境搭建
### 1、Go安装及配置
1. 进入[下载地址](https://studygolang.com/dl),下载相应版本的安装包，并进行安装
2. 安装完成后将go相关bin目录添加至`path`变量中，然后在命令行中执行`go version`,判断是否安装成功
3. 执行如下命令设置go mod相关配置
```
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```

### 2、VS Code配置
1. 搜索插件`Go`，并进行安装
2. 搜搜插件`Code Runner`，并进行安装
