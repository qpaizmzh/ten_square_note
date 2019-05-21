# 1、linux中的docker一些常用的命令

* 启动docker程序：systemctl start docker
* 查询docker中已经下载的镜像：docker images
* 创建新的docker容器：docker run -di --name=tensquare\_mysql -p 3306:3306 -e MYSQL\_ROOT\_PASSWORD=itcast  centos/mysql-57-centos7
* 查看所有创建过的docker容器：docker ps -a
* 查看正在运行的docker容器：docker ps
* 移除某一个已经创建的docker容器：docker rm 5d9317491efe\(这个是查出来的container\_id\)
* 启动指定的docker容器：容器名：`docker start docker_run`，或者ID：docker start 43e3fef2266c
* 停止某个正在运行的docker容器：docker stop \[NAME\]/\[CONTAINER id\] 强制终止：docker kill \[NAME\]/\[CONTAINER id\]



