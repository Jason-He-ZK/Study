## CentOS7

### 换源

mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
yum clean all 
yum makecache
yum -y update


### JDK

**卸载与安装**
yum install java-1.8.0-openjdk* -y

### MySQL

**卸载与安装**
wget http://repo.mysql.com/mysql80-community-release-el7-5.noarch.rpm
yum localinstall mysql80-community-release-el7-5.noarch.rpm
yum repolist enabled | grep "mysql.*-community.*"
yum install -y mysql-community-server
等挺久

**控制**
systemctl start mysqld
systemctl status mysqld
**配置**

显示随机密码
grep 'temporary password' /var/log/mysqld.log
登录
mysql -u root -p
查看密码策略
SHOW VARIABLES LIKE 'validate_password%';
修改密码策略v
set global validate_password.policy=0;
set global validate_password.special_char_count=0;
修改密码
ALTER USER 'root'@'localhost' IDENTIFIED BY '12345678';
开放远程访问
create user 'root'@'%' identified by 'Hzk0923jason';
grant all privileges on *.* to 'root'@'%' with grant option;

### Redis

**卸载与安装**
yum install redis
<!-- systemctl enable redis.service  ？？？ -->

**控制**
systemctl start redis
systemctl status redis
systemctl stop redis

**配置**
修改密码
vim /etc/redis.conf
requirepass 12345678
允许远程访问
bind 127.0.0.1 改成 bind 0.0.0.0

### Nginx

**卸载与安装**
yum install nginx

**控制**
systemctl start redis

**配置**
vim /etc/nginx/nginx.conf


### Tomcat

**安装**
（版本号可能需要修改）
wget https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-9/v9.0.65/bin/apache-tomcat-9.0.65.tar.gz
tar xvf apache-tomcat-9.0.65.tar.gz -C /usr/local/
mv /usr/local/apache-tomcat-9.0.65/ /usr/local/tomcat/

**控制**
sh /usr/local/tomcat/bin/startup.sh
sh /usr/local/tomcat/bin/shutdown.sh

**配置**
vim /usr/local/tomcat/conf/server.xml








### Docker
https://blog.csdn.net/zhanyufeng888/article/details/107831242



