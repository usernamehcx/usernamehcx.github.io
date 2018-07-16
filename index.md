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

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/usernamehcx/usernamehcx.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
