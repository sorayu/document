## 常用命令（待补充）

##### **端口**

* 端口所有：netstat -ntlp
* 指定端口：netstat -ntlp | grep 80
* 服务及端口：netstat  -lanp
* 服务的端口：ps -ef |grep mysqld

##### **文件和目录**

* 目录跳转：cd
* 删除文件：rm
* 删除目录：rm -r
* 编辑文件：vi
* 查看文件：tail | -n | -f
* 重命名：mv name1 name2 （将name1 改为 name2）
* 复制：cp | -r fileName targetDirectory
* 目录列表：ls
* 目录列表详情：ll

##### **进程**

* jvm进程：jps | -l | -v
* 所有进程：ps | -ef | grep PID
* 进程详情：ll /proc/PID

##### **常规**

* 目标路径补全：tab键
* 当前目标路径：pwd
* 磁盘大小：df
* 运行sh：./

##### **工具**

* [lrzsz工具下载及使用](https://blog.csdn.net/qq_24621911/article/details/78909634) 
* [zip工具下载及使用](https://blog.csdn.net/qq_32863631/article/details/78614037)

## 问题及解决方法

##### **liunx系统进入(initramfs)模式，修复系统**

	1. fsck -y /dev/vda1运行此命令
	2. 一路Y下去

## 环境部署

##### mysql 

---

    1.wget http://oss.aliyuncs.com/aliyunecs/onekey/mysql/mysql-5.5.35-linux2.6-x86_64.tar.gz
    2.tar zxvf mysql-5.5.35-linux2.6-x86_64.tar.gz -C /目标目标路径/
    3.cd /目标目录
    4.mv mysql-5.5.35-linux2.6-x86_64 mysql
    5.groupadd mysql
    6.useradd -g mysql -s /sbin/nologin mysql
    7./目标目标路径/mysql/scripts/mysql_install_db --datadir=/目标目标路径/mysql/data/ --basedir=/目标目标路径/mysql --user=mysql
    8.chown -R mysql:mysql /目标路径/mysql/
    9.chown -R mysql:mysql /目标路径/mysql/data/
    10.chown -R mysql:mysql /目标路径/log/mysql
    11.\cp -f /目标路径/mysql/support-files/mysql.server /etc/init.d/mysql
    12.sed -i 's#^basedir=$#basedir=/目标路径/mysql#' /etc/init.d/mysql
    13.sed -i 's#^datadir=$#datadir=/目标路径/mysql/data#' /etc/init.d/mysql
    14.\cp -f /目标路径/mysql/support-files/my-medium.cnf /etc/my.cnf
    15.sed -i 's#skip-locking#skip-external-locking\nlog-error=/目标路径/log/mysql/error.log#' /etc/my.cnf
    16.chmod 755 /etc/init.d/mysql
    17.ln -s /目标路径/mysql/bin/mysql /usr/bin
    18.ln -s /目标路径/mysql/bin/mysqladmin /usr/bin
    19./目标路径/mysql/bin/mysqladmin -u root password '密码'
    20.mysql -uroot -密码
    21.GRANT ALL PRIVILEGES ON *.* TO root IDENTIFIED BY "abc123abc";
    
##### 配置问题

######问题1：

* 在shell中输入mysql提示：The package 'mysql' can be found in following packages。

######解决方法：

* 原因：没有将mysql的路径加入环境变量
* 办法：修改/etc/profile文件，在文件末尾添加，PATH=/路径/mysql/bin:$PATH export PATH 关闭文件并生效 source /etc/profile

######问题2：

* 启动mysql服务提示：Unit mysql.service failed to load: No such file or directory。

######解决方法：

* 办法：在shell中输入：systemctl enable mysql.service

#### 导出：

	cd 到mysql安装目录中的bin文件夹.	

##### 1.导出数据库及数据

	2. mysqldump -u 用户名 -p 数据库名 > /路径/导出文件名.sql  此路径可省去,
	如果省去路径文件将在mysql安装目录中的bin文件夹中生成.

##### 2.导出一张数据表

	mysqldump -u 用户名 -p 数据库名 表名> /路径/导出的文件名.sql 路径同上.

##### 3.导出一个数据库结构

	mysqldump -u 用户名 -p -d --add-drop-table 数据库名 >/路径/导出文件名.sql 路径同上,
	-d不要数据--add-drop-table在每个create语句之前增加一个drop table.

#### 导入：

	1. 进入mysql系统：先create database 数据库名； 此数据库为导出时数据库名.
	2. use 数据库;
	3. source \路径\文件名.sql，后面不需要加分号,此路径为文件所在目录路径.


---
