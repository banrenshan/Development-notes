#1.win10安装mysql-5.7.18

##1.1配置环境变量
MYSQL_HOME:mysql安装目录
Path:mysql安装目录下面的bin子目录

##1.2修改配置文件
在mysql的安装目录下面新建my.ini文件，增加内容如下：
``
[mysqld]
basedir=D:\develop\mysql-5.7.18-winx64
datadir=D:\develop\mysql-5.7.18-winx64\data
port = 3306

``
##1.3安装mysql成系统服务
进入mysql bin目录，执行mysqld install

##1.4初始化MySQL
进入mysql bin目录，mysqld --initialize-insecure 

##1.5启动mysql服务
net start mysql 

##1.6修改管理员密码

mysqladmin -u root password 1234