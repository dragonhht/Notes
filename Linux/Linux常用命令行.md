# Linux常用命令

## 安装命令
- deb
  `sudo  dpkg  -i  *.deb`

- apt-get
   `sudo apt-get install xxx`

- make install源代码安装

    1.解压缩: `tar -zxf nagios-4.0.2.tar.gz`

   	2.进入目录: `cd nagios-4.0.2`

   	3.配置: `./configure --prefix=/usr/local/nagios`

   	4.编译: `make all`
   	5.安装: `make install && make install-init && make install-commandmode && make install-config`

- 以root权限打开文件管理: `sudo nautilus`

- 查看已安装的应用: `dpkg --list`

- 彻底卸载应用: `sudo apt-get --purge remove 应用名`

- 只卸载程序，保留配置文件: `sudo apt-get remove 应用名`


- MySQL数据库
  1，安装
  >	sudo apt-get update   
  >	sudo apt-get install mysql-server mysql-client


2，检查服务是否启动
>		sudo netstat -tap | grep mysql    （启动显示cp 0 0 localhost.localdomain:mysql *:* LISTEN - ）

3，启动服务
>		sudo /etc/init.d/mysql restart 

4，进入mysql
>		mysql -uroot -p  
>		 管理员密码 

5 , 修改编码
> cd /etc/mysql  
> vi my.cnf  
> 添加  
> [mysqld]  
> collation-server = utf8_unicode_ci  
> init-connect='SET NAMES utf8'  
> character-set-server = utf8  
> skip-character-set-client-handshake  
>
> [client]  
> default-character-set   = utf8  
>
> [mysql]  
> default-character-set   = utf8  

- 安装mysql workbench（客户端）
  >sudo dpkg -i mysql-workbench-community-6.3.5-1ubu1504-amd64.deb


- 配置jdk

  - 下载

  > curl -L "http://download.oracle.com/otn-pub/java/jdk/8u161-b12/2f38c3b165be4555a1fa6e98c45e0808/jdk-8u161-linux-x64.tar.gz" -H "Cookie: oraclelicense=accept-securebackup-cookie"  -H "Connection: keep-alive" -O

> sudo gedit /etc/profile  
> export JAVA_HOME=/opt/jvm/jdk1.8.0_102  
> export JRE_HOME=${JAVA_HOME}/jre  
> export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
> export PATH=${JAVA_HOME}/bin:$PATH  
> sudo update-alternatives --install /usr/bin/javac javac /opt/jvm/jdk1.8.0_102/bin/javac 300  
> sudo update-alternatives --install /usr/bin/java java /opt/jvm/jdk1.8.0_102/bin/java 300

- 配置tomcat（tomcat放在home下，不然会没有权限）
> export CATLINA_HOME=/opt/jvm/apache-tomcat-8.0.37  
> sudo chmod -R 755 /opt/jvm/apache-tomcat-8.0.37

- Ubuntu与Windows10时间相差8小时的解决
  >timedatectl set-local-rtc true 

- 建立桌面快捷方式
  1, 运行
> sudo gedit  /usr/share/applications/小书匠.desktop

2,文件写入
> [Desktop Entry]  
> Encoding=UTF-8  
> Name=小书匠  
> Comment=story-writer  
> Exec=/home/huang/application/Story-writer-linux64/Story-writer  
> Icon=/home/huang/application/Story-writer-linux64/Story-writer.png  
> Terminal=story-writer  
> Notify=true  
> Type=Application  
> Categories=Application;Development;  



# 设置远程连接服务器MySQL

1. 修改`/etc/mysql/mysqld.cnf` 注释 `bind-address = 127.0.0.1` 
2. 在MySQL中添加也有任意ip访问的用户 `GRANT ALL PRIVILEGES ON *.* TO ‘huang'@'%' IDENTIFIED BY 'admin' WITH GRANT OPTION` 

# 创建交换分区

- 创建4G大小的交换文件:`sudo fallocate -l 4G /swapfile`

- 赋予600权限： `sudo chmod 600 /swapfile`

- 将创建的文件转换为交换文件: `sudo mkswap /swapfile`

- 使交换文件生效: `sudo mkswap /swapfile`

- 将创建的交换文件添加到`fstab`中，使交换文件永久有效: `vim /etc/fstab`, 添加：`/swapfile       swap            swap    defaults          0       0`

# Deepin更换源

- `sudo gedit  /etc/apt/sources.list`

- 将`http://packages.deepin.com`替换为想要的源，如阿里云`https://mirrors.aliyun.com`

- `sudo apt update`