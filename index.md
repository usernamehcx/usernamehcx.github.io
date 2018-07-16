## Welcome to Username's GitHub Pages

这是我本人的第一篇博客，主要是介绍scrapyd爬虫部署。

## scrapyd
之前通过scrapy写了几个比较简单的爬虫，主要熟悉了一下scrapy的爬虫框架。但是之前的爬虫都是在本地运行，如果考虑到实际环境（例如我们需要跑多个爬虫任务的时候，单台机器运行的效率较低），我们可能需要把爬虫部署到不同的机器上运行，实现一个分布式的爬虫。

目前分布式爬虫的设计大多数都是基于scrapy-redis框架设计的，采用主从结构，把一台机器作为master用来管理redis数据库，以及调度request，而其他的机器作为slave，在上面部署爬虫解析网页并提取数据。

### scrapyd-client

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

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
