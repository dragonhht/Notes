# MySQL的主从配置

## 1、在master上创建其他IP可访问的账号

-   创建账号: `CREATE USER 'slave'@'%'IDENTIFIED BY '密码';`

-   授权: `GRANT ALL PRIVILEGES ON *.* TO 'slave'@'%';`

## 在master和slave的配置文件中开启二进制日志及配置ID

```
[mysqld]
# 启用进制日志
log-bin=mysql-bin
# 配置服务器唯一ID，一般去IP最后一段
server-id=121
```

-   重启服务：`service mysql restart`

## 在master节点查看master状态

-   进入MySQL客户端，输入命令: `show master status;`

-   记录结果

![master status](https://github.com/dragonhht/GitImgs/blob/master/database/mysql_master_status.png?raw=true)

## 在slave节点配置master信息

-   进入MySQL客户端,输入命令:`change master to master_host='192.168.23.128',master_user='slave',master_password='123',master_port=3308,master_log_file='mysql-bin.000001',master_log_pos=154;`

-   说明

    -   `master_host`: master节点的IP

    -   `master_user`: master上可远程连接的账号

    -   `master_password`：账号密码

    -   `master_port`: master服务所在的端口

    -   `master_log_file`: 上一步查看的master信息中的日志文件

    -   `master_log_pos`: 上一步查看的master信息中的日志文件位置

-   启动slave节点: `start slave;`

-   查看slave节点信息: `show slave status\G;`,当信息中`Slave_IO_Running`和`Slave_SQL_Running`都为`Yes`则说明配置成功

![slave信息](https://github.com/dragonhht/GitImgs/blob/master/database/slave_status.png?raw=true)
