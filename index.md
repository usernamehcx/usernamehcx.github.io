## Welcome to Username's GitHub Pages

这是我本人的第一篇博客，主要是介绍scrapyd爬虫部署。

## scrapyd
之前通过scrapy写了几个比较简单的爬虫，主要熟悉了一下scrapy的爬虫框架。但是之前的爬虫都是在本地运行，如果考虑到实际环境（例如我们需要跑多个爬虫任务的时候，单台机器运行的效率较低），我们可能需要把爬虫部署到不同的机器上运行，实现一个分布式的爬虫。

目前分布式爬虫的设计大多数都是基于scrapy-redis框架设计的，采用主从结构，把一台机器作为master用来管理redis数据库，以及调度request，而其他的机器作为slave，在上面部署爬虫解析网页并提取数据。问题是它是以request为调度对象，不是以每个spider进行调度的，无法对运行在每个机器上的爬虫进行调度。

如果用scrapyd的话，我们能够远程启动，停止，删除机器上运行spider，但无法对于每个爬虫进行一些复杂的调度。不过我们可以把scrapyd看作是一个执行器，通过将scrapyd将爬虫任务发布到各个主机，然后通过scrapyd启动爬虫。在执行器的外层通过设计一个mesos的调度器进行调度，由这个调度器来决定在哪个主机上运行什么样的任务，做一个决策，然后在这个确定的主机上让scrapyd执行器来启动运行这个任务。这样来实现一个简单的分布式爬虫的设计。

下面先简单介绍一下如何用scrapyd进行部署。

**安装scrapyd**
```shell
pip install scrapyd
```

## scrapyd-client

**安装scrapyd-client**
```
pip install scrapyd-client
```
**运行scrapyd**
```
scrapyd
```
 此时我们可以scrapyd回监听localhost：6800端口，6800端口可以访问。

**修改scrapy.cfg**
```python
[settings]
default = yourspider.settings

[deploy] 
url = http://localhost:6800/
project = yourspider 
```
将原来的scrapy.cfg中“#url = http: //localhost:6800/ ”中的“#”号去掉

**发布爬虫**
```
scrapyd-deploy -p yourspider
```
在项目的根目录执行该命令，scrapyd-deploy会将项目打包为.egg文件，并将打包后的文件部署在scrapyd上

**创建爬虫任务**
```
curl http://localhost:6800/schedule.json -d project=myproject -d spider=spider1
```
启动项目中的spider1，通过http://localhost:6800/jobs 可以查看爬虫任务，可以通过日志查看爬虫运行情况。
如果需要其他的停止，删除爬虫的API可以查看[Scrapyd JSON API](http://scrapyd.readthedocs.io/en/latest/api.html)
