title: brew install mysql on mac os
date: 2015-11-22 13:15:29
tags:
---
### 1. 安装
	$ brew install mysql

### 2. 设置MySQL数据存放地址
	$ vi /etc/my.cnf
输入以下内容：
	[mysqld]
	datadir=/usr/local/var/mysql
	plugin_dir=/usr/local/Cellar/mysql/5.7.9/lib
	basedir=/usr/local/Cellar/mysql/5.7.9
	pid-file=/usr/local/var/mysql/xpMac.local.pid
	log-error=/usr/local/var/mysql/xpMac.local.err

### 3. 修改root密码
	$ ps -ef|grep mysqld	(查看所有mysqld进程)
	$ kill -9 123	(关闭所有mysqld进程，123为进程id)
	$ mysqld_safe --skip-grant-tables
	$ mysql -uroot
	$ update user set authentication_string=password('1111') where user='root’;

### 4. 让MySQL开机自启动
	$ mkdir -p ~/Library/LaunchAgents
	$ ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
	$ find /usr/local/Cellar/mysql/ -name "homebrew.mxcl.mysql.plist" -exec cp {} ~/Library/LaunchAgents/ \;
	$ launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist

### 5. 登陆MySQL客户端
	$ mysql -uroot -p 
	输入刚才设置的密码就可以了

### 6. 安装MySQL Workbench，一款专为MySQL设计的ER/数据库建模工具
	下载地址：http://dev.mysql.com/downloads/workbench/


### 参考
* http://stackoverflow.com/questions/4359131/brew-install-mysql-on-mac-os
* http://stackoverflow.com/questions/30692812/mysql-user-db-does-not-have-password-columns-installing-mysql-on-osx/31122246#31122246 
* http://blog.neten.de/posts/2014/01/27/install-mysql-using-homebrew/