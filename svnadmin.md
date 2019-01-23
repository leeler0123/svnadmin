#svnadmin 搭建

svnadmin是由java+tomcat+mysql+svn组合构成，本人系统是centos7+mysql5.6+svn1.7+jdk8

本人统一将源码包安装到/usr/local/src，软件安装到/usr/local下

##1.下载jdk
`# wget http://download.oracle.com/otn-pub/java/jdk/8u191-b12/2787e4a523244c269598db4e85c51e0c/jdk-8u191-linux-x64.tar.gz?AuthParam=1540367779_02b45d1d388213f5c26ac8837da3abe7`

解压

`# tar zxvf jdk-8u191-linux-x64.tar.gz\?AuthParam\=1540353430_f65dbaa1cedb55bd47708b24615ed931`

移动软件包到/usr/local/

`# mv jdk1.8.0_191/ /usr/local/jdk`

配置jdk环境变量

`# vim /etc/profile`

将以下代码写在文件末尾


export JAVA_HOME=/usr/local/jdk

export CLASSPATH=.:${JAVA_HOME}/jre/lib/rt.jar:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar

export PATH=$PATH:${JAVA_HOME}/bin


CentOS6上面的是JAVAHOME，CentOS7是{JAVA_HOME}

配置生效：`# source /etc/profile`
有提示版本就标识成功：`# java -version`


***

##2.下载tomcat
`# wget http://mirrors.shu.edu.cn/apache/tomcat/tomcat-9/v9.0.12/bin/apache-tomcat-9.0.12.tar.gz`

解压
`# tar zxvf apache-tomcat-9.0.12.tar.gz`

移动软件包到/usr/local/
`# mv apache-tomcat-9.0.12 /usr/local/tomcat`

`# vim /etc/profile`

将以下代码写在文件末尾

export TOMCAT_HOME=/usr/local/tomcat

export CATALINA_HOME=/usr/local/tomcat

配置生效：`# source /etc/profile`

启动tomcat:`#vim /usr/local/tomcat/bin/startup.sh`

在浏览器输入:http:IP地址：8080 ，出现tomcat那只猫首页就说明安装启动成功

关闭tomcat:`#vim /usr/local/tomcat/bin/shutdown.sh`

***
##3.创建数据库表

创建数据库
`create database if not exists svnadmin default character set utf8;`

导入db/svnadmin.sql文件创建表

##4.导入svnadmin项目
修改svnadmin.war/WEB-INF/jdbc.properties里的数据库连接信息

将svnadmin 项目导入tomcat服务器根目录
`mv svnadmin.war /usr/local/tomcat/webapps/`

在浏览器输入http://IP地址:8080/svnadmin 则进入svnadmin首页

第一次输入用户名和密码为初始密码，请记住！

这里有两个账户，一个是svnadmin后台账户，一个是仓库用户，账户共用一个用户名。密码存在两种，一种是后台密码，一种是仓库密码。右上角用户是管理所有用户的，可删除用户和修改用户的后台密码。仓库密码修改需要点击右上角仓库，设计用户即可修改。

添加仓库步骤：

* 点击右上角项目，出现项目管理页面（这里只可以选svn类型，因为没装apache,不支持http协议）
* 分别输入仓库文件夹名(leeler)、项目路径(\data\svn\leeler)、URL（包括仓库名svn://IP地址：8080/仓库名）,描述
* 管理权限，右上角添加用户，然后在对应的仓库项目设置用户仓库密码，设置用户组，设置权限

注意，只有超级管理员才可以管理仓库，设置权限时访问路径显示没授权，就必须把管理员设置到跟目录有读写权限的组去，用户即可svn检出目录了。

如还有不清楚的可查看SvnAdmin_Manual_zh_CN.pdf，官方中文手册。
