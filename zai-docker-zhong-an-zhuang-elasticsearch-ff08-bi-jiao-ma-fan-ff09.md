# 在docker中安装elasticSearch

在linux的centos中安装elasticsearch很麻烦，需要看好

---

## 安装elasticSearch

* 从docker中下载elasticSearch插件，并按照以下的命令创建容器：docker run -di --name=tensquare\_elastic\_Search -p 9200:9200 -p 9300:9300 elasticsearch:5.6.8\(不知道为什么设置两个端口\)
* 链接之后就会出现不能访问的情况，这是elasticSearch的配置文件中不允许远程访问，要重新对容器的配置文件进行设置：                                           1、使用命令：docker exec -it tensquare\_elastic\_Search  /bin/bash 这里会进入该容器的根路径，在原来的基础上输入ls，就可以看到下一层的文件夹configs,再从configs文件夹里面找到配置文件                                                                                                                     2、因为XFTP软件只能访问到宿主机的/usr/shares/的文件夹，不能访问到容器内部，首先把容器的文件复制到前面提到的路径:首先exit退出容器路径访问，接着输入docker cp  容器名：（容器里配置文件的路径）：（复制文件的到达指定路径） 例子： docker cp tensquare\_elasticsearch:/usr/share/elasticsearch/config/elasticsearch.yml /usr/share/elasticsearch.yml                   3、删除原来的容器，在docker中使用stop命令和rm命令                                                                                                                                       4、修改配置文件，可以在docker访问到该文件，然后使用vi进行编辑，按i键进入编辑模式，主要是把host那行的井号去掉，目的是可以让任意的IP进行访问，编辑完成后先esc退出，然后shift+z+z保存并退出
* 重启容器还是会出现问题，因为系统判断你放开了IP限制，需要更多的资源提供，所以还要修改系统一些相关的配置：                                  1、使用vi，进入/etc/security/limits.conf，追加两行内容，放开硬件和软件的限制：\* soft nofile 65536 \* hard nofile 65536  （ nofile是单个进程允许打开的最大文件个数 soft nofile 是软限制 hard nofile是硬限制）                                                                                   2、 修改/etc/sysctl.conf，追加内容， vm.max\_map\_count=655360， 这可以限制一个进程可以拥有的VMA\(虚拟内存区域\)的数量，然后使用 sysctl ‐p 使参数立马生效，然后重启宿主机，重启服务 应该就可以了

## 安装IK分词器

## 安装header插件



