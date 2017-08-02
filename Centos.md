
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
> rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.71-2.5.3.1.el7_0.x86_64 

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

安装redis 
----------

