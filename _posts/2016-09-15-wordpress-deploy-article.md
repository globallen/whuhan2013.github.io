---
layout: post
title: WordPress自动抓取发布文章 
date: 2016-9-15
categories: blog
tags: [python]
description: WordPress自动抓取发布文章
---


很多用WordPress建站的朋友都有这样的苦恼，网站建好了，没有时间自己写文章，慢慢就荒废了，还有的朋友在浏览器收集好多喜欢的博客网站地址，因为收集的网址太多太杂，从此也很少点开看。其实只要几行代码我们就可以完全利用Python和WordPress建一个属于自己的文章抓取站点。主要是运用python newspaper xmlrpc 模块编写实现网页爬虫，通过正则匹配爬取网页内容后，用xmlrpc自动发布到WordPress部署的网站。然后采用crond定时抓取。

- python抓取URL
- newspaper解析页面
- xmlrpc上传到wordpress


```
#/usr/bin/env python
#coding=utf8
import httplib
import hashlib
import urllib
import random
import urllib2
import md5
import re
import json
import sys
import time
from lxml import html
from wordpress_xmlrpc import Client, WordPressPost
from wordpress_xmlrpc.methods.posts import NewPost
from newspaper import Article
reload(sys)
sys.setdefaultencoding('utf-8')
time1 = time.time()
#得到html的源码
def gethtml(url1):
    #伪装浏览器头部
    headers = {
       'User-Agent':'Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US; rv:1.9.1.6) Gecko/20091201 Firefox/3.5.6'
    }
    req = urllib2.Request(
    url = url1,
    headers = headers
    )
    html = urllib2.urlopen(req).read()
    return html
#得到目标url源码
code1 = gethtml('http://whuhan2013.github.io/archive/')

tree = html.fromstring(code1)
#print tree

targeturl=tree.xpath("//li[@class='listing-item']/a/@href")

def sends():
    # print targeturl
    for i in range(len(targeturl)):
        #u=content1[i][0]
        url="http://whuhan2013.github.io"+targeturl[i]
        print url
        a=Article(url,language='zh')
        a.download()
        a.parse()
        #print a.text
        dst=a.text
        tag='test'
        title=a.title
        #print 'here2'
   
        #链接WordPress，输入xmlrpc链接，后台账号密码
        wp = Client('http://119.29.152.242/wordpress/xmlrpc.php','Ricardo','286840jjx')
		#示例：wp = Client('http://www.python-cn.com/xmlrpc.php','username','password')
        post = WordPressPost()
        post.title = title
        # post.post_type='test'        
        post.content = dst
        post.post_status = 'publish'
        #发送到WordPress
        #print 'here3'
        wp.call(NewPost(post))
        time.sleep(3)
        print 'posts updates'

if __name__=='__main__':
    sends()
    f1.close()
```


最后，可以通过crontab定时运行程序，采集指定文章发送到WordPress

**参考链接：**[运用Python实现WordPress网站大规模自动化发布文章](https://zhuanlan.zhihu.com/p/22261238)

**源码：**[wordpress自动发布](https://github.com/whuhan2013/pythoncode/tree/master/wordpress)

**访问：**[良有以也的博客](http://119.29.152.242/wordpress/)   

**wordpress支持Markdown与代码高亮，丰富文章样式，文章访问量插件等**   

[给力星的博客插件](http://www.powerxing.com/about/)

**效果如下**

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/php/p12.png)

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/php/p13.png)





