
Centos7.2下部署项目
==================
安装jdk1.8
---------------
1. 查看系统是否安装过jdk 
>rpm -qa | grep java  


如果安装过jdk，会出现类似下面的语句
> avapackages-tools-3.4.1-6.el7_0.noarch  
> tzdata-java-2014i-1.el7.noarch  
> java-1.7.0-openjdk-headless-1.7.0.71-2.5.3.1.el7_0.x86_64 
> java-1.7.0-openjdk-1.7.0.71-2.5.3.1.el7_0.x86_64  
> python-javapackages-3.4.1-6.el7_0.noarch  

此时使用以下语句逐一卸载  
>rpm -e --nodeps tzdata-java-2014i-1.el7.noarch 
>rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.71-2.5.3.1.el7_0.x86_64 

2. 安装新的jdk1.8 

创建安装路径  
> mkdir  -p /usr/lib/jvm  

解压到新建的安装路径中 
> tar  -zxvf jdk1.8.0_31.tar.gz -C  /usr/lib/jvm  

3. 设置环境变量 
> vi  /etc/profile  

在最后添加一下配置(JAVA_HOEM的路径以实际安装路径为准) 
>export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_31  
>export JRE_HOME=${JAVA_HOME}/jre   
>export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
>export  PATH=${JAVA_HOME}/bin:$PATH  

4. 执行profile  
>source /etc/profile  

5. 查看新安装的jdk版本  
>java -version


安装mariadb 
----------  
1. 查看是否安装过mariadb  
>rpm -qa | grep maria*  

如果出现以下语句  
>MariaDB-server-5.5.49-1.el7.centos.x86_64  
>MariaDB-common-5.5.49-1.el7.centos.x86_64  
>MariaDB-client-5.5.49-1.el7.centos.x86_64  

说明安装过mariadb，执行以下语句，卸载mariadb数据库  
>yum -y remove mari*  

执行以下语句，删除数据库文件  
>rm -rf /var/lib/MySQL/*  

2. 安装新的mariadb 
>yum -y install mariadb mariadb-server  

3. 启动mariadb服务 
>systemctl start mariadb  

4. 设置系统开启自启动  
>systemctl enable mariadb 

5. 修改配置文件 
>vim /etc/my.conf 

在下述语句后面添加配置 
>[mysqld]   
>datadir=/var/lib/mysql   
>socket=/var/lib/mysql/mysql.sock   

设置字符集为utf8  
>character-set-server=utf8  
>collation-server=utf8_bin  

设置sql_model 
>sql_mode="STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION"  

6. mariadb第一次启动时，初始root密码为空，根据提示或者执行以下语句修改密码   
>mysqladmin  -uroot -p  password '新密码'  

7. 导入sql文件  
进入mariadb，新建与文件名相同的数据库  
>create database Student; 

选中新建的数据库  
> use Student;  

设置默认编码  
>set names utf8;  

输入数据库所在路径 
>source /usr/database.sql;  

  
安装redis 
----------

1. 安装GCC和Make，在编译时会用到 
>yum install gcc make 

2. 从官网下载redis安装包  
>curl http://download.redis.io/releases/redis-3.0.4.tar.gz -o redis-3.0.4.tar.gz  

3. 解压缩  
>tar zxvf redis-3.0.4.tar.gz  

4. 进入解压后的redis目录，进行编译 
>cd redis-3.0.4   
>make   

5. 编译完成之后，在src目录下有2个重要程序生成，一个是redis-server，另一个是redis-cli；接着进入src目录，执行make install，这时会把这些可执行程序拷贝到/usr/local/bin目录下，由于/usr/local/bin是在系统的环境变量$PATH下定义的，因此终端在任意位置就可以执行redis-server和redis-cli了 
>cd src/  
>make install 

6. 把redis目录下的redis.conf文件拷贝到/etc/redis/6379.conf 
>cd /etc  
>mkdir redis  
>cp /usr/redis/redis.conf /etc/redis/6379.conf 

7. 将redis_init_script脚本拷贝到/etc/init.d/redisd 
>cp /usr/redis/utils/redis_init_script /etc/init.d/redisd 

8. 启动、关闭redis服务 
>service redisd start  
>service redisd stop  

安装wildfly 
------------ 

1. 解压安装wildfly 
>unzip  wildfly-10.0.0.Final.zip  -d  /usr/local/  
>mv /usr/local  
>mv wildfly-10.0.0.Final wildfly 

2. 修改配置文件standalone.xml，将文件中的127.0.0.1替换成0.0.0.0 
>vim /usr/local/wildfly/standalone/configuration/standalone.xml 

3. 然后启动服务 
>cd /wildfly/bin  
>./standalone.sh






