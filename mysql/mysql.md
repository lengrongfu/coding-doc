# mysql centos7 安装指南(Yum)

- 参考文档：[https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/](https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/)
### 1、添加　mysql yum存储库
- 下载平台发行包：`wget -c https://repo.mysql.com//mysql57-community-release-el6-11.noarch.rpm` mysql5.7版本
- 安装下载的发行包：`$ sudo rpm -Uvh mysql57-community-release-el6-n.noarch.rpm`
### 2、选择版本
- 默认情况下选择最新的mysql版本进行安装，如果这是能接收的那就跳到安装步骤。
- 可以查看启用或禁用的版本：`shell > yum repolist all | grep mysql`，执行结果，默认最新版启用的。
```shell
Repository epel is listed more than once in the configuration
!mysql-connectors-community/x86_64 MySQL Connectors Community       启用:     36
mysql-connectors-community-source  MySQL Connectors Community - Sou 禁用
!mysql-tools-community/x86_64      MySQL Tools Community            启用:     47
mysql-tools-community-source       MySQL Tools Community - Source   禁用
mysql-tools-preview/x86_64         MySQL Tools Preview              禁用
mysql-tools-preview-source         MySQL Tools Preview - Source     禁用
mysql55-community/x86_64           MySQL 5.5 Community Server       禁用
mysql55-community-source           MySQL 5.5 Community Server - Sou 禁用
mysql56-community/x86_64           MySQL 5.6 Community Server       禁用
mysql56-community-source           MySQL 5.6 Community Server - Sou 禁用
!mysql57-community/x86_64          MySQL 5.7 Community Server       启用:    187
mysql57-community-source           MySQL 5.7 Community Server - Sou 禁用
```
- 启用自定义版本并禁用最新版：
```shell
shell> sudo yum-config-manager --disable mysql57-community
shell> sudo yum-config-manager --enable mysql56-community
```

### 3、安装 `MySQL`

- 以下是安装命令：
```shell
$ sudo yum install mysql-community-server
```

### 4、启动MySQL服务器

-　使用以下命令启动MySQL：
```shell
 $ sudo service mysqld start
```
-　如果系统是`Centos7` 首选安装命令是：
```shell
$ sudo systemctl start mysqld.service
```
- 以下命令是检查MySQL服务器的状态：
```shell
$ sudo service mysqld status
```
- 对于`CentOs7`平台，首先以下命令：
```shell
$ sudo systemctl status mysqld.service
```
MySQL服务初始化(仅适用5.7)

### 5、保护MySQL（仅使用于MySQL5.6）
- 初始化设置MySQL密码，执行以下命令：
```shell
$ mysqladmin -u root password
```
### ６、安装其他的MySQL工具
- 使用yum来安装和管理MySQL的各个组件，使用以下命令从mysql yum存储库中列出所有mysql：
```shell
$ yum --disablerepo=\* --enablerepo='mysql*-community*' list available
```
- 使用以下命令安装选择软件包：
```shell
$ sudo yum install package-name
//安装mysql workbench
eg: $ sudo yum install mysql-workbench-community
```

### 7、使用yum升级MySQL
*作为一般规则，要从一个版本系列升级到另一个版本，请转到下一个系列，而不是跳过一系列。例如，如果您目前正在运行MySQL 5.5并希望升级到5.7，请先升级到MySQL 5.6，然后升级到5.7。*


- 使用以下命令升级MySQL：

```shell
$ sudo yum update mysql-server
```
- 在Yum更新后，MySQL服务器会重新启动。重启之后运行`mysql_upgrade`命令检查数据和软件升级带来的不兼容问题。

### 8、创建用户和授权、修改密码
- 创建用户命令：
```mysql
#foo表示你要建立的用户名，后面的123表示密码,
#localhost限制在固定地址localhost登陆,如果没有限制则用%
CREATE USER foo@localhost IDENTIFIED BY '123';
```
- 授权命令：
```mysql
#说明: privileges - 用户的操作权限,如SELECT , INSERT , UPDATE 等。如果要授予所的权限则使用 ALL;
#databasename - 数据库名,tablename-表名,如果要授予该用户对所有数据库和表的相应操作权限则可用*表示, 如*.*
GRANT privileges ON databasename.tablename TO 'username'@'host'
eg:
GRANT INSERT,DELETE,UPDATE,SELECT ON test.user TO 'foo'@'localhost';
flush privileges;
```
- 设置与更改用户密码：
```mysql
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword')

#如果是当前登陆用户
SET PASSWORD = PASSWORD("newpassword");

#例如：
SET PASSWORD FOR 'foo'@'%' = PASSWORD("123456");

update mysql.user set password=password('新密码') where User="phplamp" and Host="localhost";
```