# 1、linux中的docker一些常用的命令

* 启动docker程序：systemctl start docker
* 查询docker中已经下载的镜像：docker images
* 创建新的docker容器：docker run -di --name=tensquare\_mysql -p 3306:3306 -e MYSQL\_ROOT\_PASSWORD=itcast  centos/mysql-57-centos7
* 查看所有创建过的docker容器：docker ps -a
* 查看正在运行的docker容器：docker ps



