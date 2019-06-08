# 在docker中安装elasticSearch

在linux的centos中安装elasticsearch很麻烦，需要看好

---

## 安装elasticSerach

* 从docker中下载elasticSearch插件，并按照以下的命令创建容器：docker run -di --name=tensquare_elastic_Search -p 9200:9200 -p 9300:9300 elasticsearch:5.6.8\(不知道为什么设置两个端口\)



