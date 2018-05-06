---
title: hexo的SEO方法
date: 2018-01-23 21:23:59
tags: [SEO,hexo,搜索引擎优化]
categories: [网络]
---
最近，CSDN总不停变风格，而且越变越丑，于是搭了这个博客。然而，流量实在太小！所以决定做SEO（搜索引擎优化），就当是多学些东西吧。参考网上一些教程，总结了Hexo的Next主题做SEO的一些小技巧：
<!-- more -->
## Hexo优化准备
### 主站文件配置
主站配置文件中的这四项**一定要填写！**
``` yml
title:  #标题
subtitle:  #子标题
description: #描述
url: #url
```
### 打开Next主题自带的seo配置
打开**主题**配置文件中的这些项目。
```yml
canonical: true
seo: true
index_with_subtitle: true
baidu_push: true
```
未按照配置文件顺序，自行搜索。
## 首页title优化
修改文件：**your-hexo-site\themes\next\layout\index.swig**
```yml
{% block title %} {{ config.title }} {% endblock %}
```
改成
```yml
{% block title %} {{ theme.keywords }} - {{ config.title }}{{ theme.description }} {% endblock %}
```
## 添加sitemap
这一步目的在于告诉搜索引擎你的站点结构。
### sitemap生成插件的安装和配置
```yml
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
```
在站点配置文件中添加sitemap的生成路径
```yml
sitemap: 
    path: sitemap.xml
baidusitemap:
    path: baidusitemap.xml
```
现在执行 hexo g 生成以后应该可以访问sitemap.xml和baidusitemap.xml
### 提交sitemap
分别到谷歌和百度的站长工具网站上提交sitemap就可以了。
如果不主动提交sitemap，搜索引擎可能无法自己找到sitemap，即使找到，速度也会很慢。
github好像屏蔽了百度的爬虫，所以即使提交了sitemap，也可能出现无法爬下来的情况。于是我们需要主动向百度提交链接。
安装插件**hexo-baidu-url-submit**
```yml
npm install hexo-baidu-url-submit --save
```
在站点配置文件中添加如下内容：
```yml
baidu_url_submit:
  count: 1 ## 提交最新的一个链接
  host:  ## 在百度站长平台中注册的域名
  token: your_token ## 请注意这是您的秘钥， 所以请不要把博客源代码发布在公众仓库里!
  path: baidu_urls.txt ## 文本文档的地址， 新链接会保存在此文本文档里
```
在站点配置文件的deploy下仿照github的类型添加：
```yml
- type: baidu_url_submitter 
```
执行hexo d的时候就会自动推送新链接了
### robots文件
在**your-hexo-site\source**中新建robots.txt，告诉搜索引擎，哪些是可以爬的，哪些是不可以爬的，格式如下：
```yml
#hexo robots.txt
User-agent: *

Allow: /
Allow: /archives/
Allow: /categories/
Allow: /tags/

Disallow: /vendors/
Disallow: /js/
Disallow: /css/
Disallow: /fonts/
Disallow: /vendors/
Disallow: /fancybox/

Sitemap: https://hjptriplebee.github.io/search.xml
Sitemap: https://hjptriplebee.github.io/sitemap.xml
Sitemap: https://hjptriplebee.github.io/baidusitemap.xml
```
## 修改文章链接
hexo默认的url形式是按日期来的多重结构，过长的url不利于搜索。可以将站点配置文件作如下修改：
```yml
permalink: :title.html
```
## nofollow标签
可以添加的地方太多了，非友情链接都可以添加如下标签：
```yml
rel="external nofollow"
```
