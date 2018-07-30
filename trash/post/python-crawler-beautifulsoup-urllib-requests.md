---
title: Python的3种爬虫（Beautifulsoup、urllib、requests）
date: 2018-04-20 19:11:11
tags: [技术,python,学习笔记,爬虫]
---
这里，我们先不说爬虫框架Scrapy，先说一下Python的3种常用的爬虫库，[Beautifulsoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)、[urllib](https://docs.python.org/3/library/urllib.html)和[requests](http://www.python-requests.org/en/master/)。
爬虫说简单也简单，说复杂也复杂。Python上有很多优秀的爬虫库，在这里我将简单介绍这3种爬虫并进行简单的比较。顺便放一个我之前写的一个可搜索javbus、btso的磁力链接和avgle的预览视频的爬虫[iav](https://github.com/WWILLV/iav)
<!--more-->

## 介绍
### Urllib
Urllib是python内置的HTTP请求库，包括以下模块：
urllib.request 请求模块
urllib.error 异常处理模块
urllib.parse url解析模块
urllib.robotparser robots.txt解析模块

#### Python2和Python3中的Urllib
[参考](https://blog.csdn.net/drdairen/article/details/51149498)
首先是Python2的urllib与urllib2[（可见此处）](http://www.hacksparrow.com/python-difference-between-urllib-and-urllib2.html)
urllib和urllib2都是接受URL请求的相关模块，但是提供了不同的功能。两个最显著的不同如下：
1、urllib2可以接受一个Request类的实例来设置URL请求的headers。
2、urllib仅可以接受URL。这意味着，你不可以伪装你的User Agent字符串等。

Python3中把这些打包成为了2个包，就是http与urllib，详解如下：
http会处理所有客户端--服务器http请求的具体细节，其中：
（1）client会处理客户端的部分
（2）server会协助你编写Python web服务器程序
（3）cookies和cookiejar会处理cookie，cookie可以在请求中存储数据
urllib是基于http的高层库，它有以下三个主要功能：
（1）request处理客户端的请求
（2）response处理服务端的响应
（3）parse会解析url

#### urlopen
关于urllib.request.urlopen参数的介绍：
```python
urllib.request.urlopen(url, data=None, [timeout, ]*, cafile=None, capath=None, cadefault=False, context=None)
```
```python
import urllib.request
response = urllib.request.urlopen('http://www.baidu.com')
print(response.read().decode('utf-8'))
```

#### request
有很多网站为了防止程序爬虫爬网站造成网站瘫痪，会需要携带一些headers头部信息才能访问，最常见的有user-agent参数，有时还需要referer，cookie甚至是代理，这就需要request了。（可以参考我的iav，里面就有这些设置，在这里放一点）
```python
url=getAjax(avid)
proxy = urllib.request.ProxyHandler({'https': proxy_addr})
opener = urllib.request.build_opener(proxy, urllib.request.HTTPHandler)
opener.addheaders = [
    ('User-Agent',
     'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2272.101 Safari/537.36'),
    ('Host','www.javbus.com'),
    ('Connection','close'),
    ('X-Requested-With','XMLHttpRequest'),
    ('Referer',url)
]
urllib.request.install_opener(opener)
soup = BeautifulSoup(urllib.request.urlopen(url).read().decode('utf-8'), 'lxml')
```
虽然我这里最后用了beautifulsoup，但是前面的request就设置了代理和header。

#### urlparse
这个主要是用来解析URL的

### Requests
Requests是用python语言基于urllib编写的，采用的是Apache2 Licensed开源协议的HTTP库。
如果你看过上篇文章关于urllib库的使用，你会发现，其实urllib还是非常不方便的，而Requests它会比urllib更加方便，可以节约我们大量的工作。一句话，requests是python实现的最简单易用的HTTP库。
默认安装好python之后，是没有安装requests模块的，需要单独通过pip安装。
Request我用的不多，这里有一篇文章写的不错，可以参考一下。[python爬虫从入门到放弃（四）之 Requests库的基本使用](http://www.cnblogs.com/zhaof/p/6915127.html)
requests里提供各种请求方式
```python
import requests
requests.post("http://httpbin.org/post")
requests.put("http://httpbin.org/put")
requests.delete("http://httpbin.org/delete")
requests.head("http://httpbin.org/get")
requests.options("http://httpbin.org/get")
```

### Beautifulsoup
bs是我目前使用的最多的一种爬虫方法，具体原因我们后面说。
截取一段我写在iav里的一段代码
```python
soup = BeautifulSoup(urllib.request.urlopen(url).read().decode('utf-8'), 'lxml')
avdist={'title':'','magnet':'','size':'','date':''}
for tr in soup.find_all('tr'):
    i=0
    for td in tr:
        if(td.string):
            continue
        i=i+1
        avdist['magnet']=td.a['href']
        if (i%3 == 1):
            avdist['title'] = td.a.text.replace(" ", "").replace("\t", "").replace("\r\n","")
        if (i%3 == 2):
            avdist['size'] = td.a.text.replace(" ", "").replace("\t", "").replace("\r\n","")
        if (i%3 == 0):
            avdist['date'] = td.a.text.replace(" ", "").replace("\t", "").replace("\r\n","")
    print(avdist)
```
这是一个灵活又方便的网页解析库，处理高效，支持多种解析器。之前就叫BeautifulSoup，不过这个版本不再更新，后续发布bs4，包含了BeautifulSoup。pip安装后`from bs4 import BeautifulSoup`即可。
beautifulsoup是一个强大的解析库，不需要编写正则表达式就可以进行快速的爬取。具体的可以参见BeautifulSoup的[官方文档](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)。

## 3种方法的区别和优缺点
在说之前，我们先放一段代码（可能有点长）
```python
# -*- coding: UTF-8 -*-
import time
import urllib.request as request
import requests
import re
from bs4 import BeautifulSoup

def getUrl():
    url='https://willv.cn/'
    urlList=[]
    for i in range(1,3):
        if not i==1:
            url=(url+'page/'+str(i)+'/')
        html_doc = request.urlopen(url).read().decode('utf-8')
        soup = BeautifulSoup(html_doc, 'html.parser')
        urlList.append(url)
        url = 'https://willv.cn/'
    return urlList


def bs(urllist):
    count=0
    for url in urllist:
        html_doc = request.urlopen(url).read().decode('utf-8')
        soup = BeautifulSoup(html_doc, 'html.parser')
        for link in soup.find_all(class_='post-title-link'):
            print(link.text+'('+link['href']+')')
            count=count+1
    print('共'+str(count)+'篇文章')


def ul(urllist):
    count=0
    reText=r'<a class="post-title-link" href=".*?" itemprop="url">.*?</a>'
    reResult=[]
    for url in urllist:
        response = request.urlopen(url)
        html=response.read().decode('utf-8')
        reResult.extend(re.findall(reText,html))
    for result in reResult:
        reHref=r'href=".*?"'
        retxt=r'>.*?</'
        href=re.search(reHref,result)
        txt=re.search(retxt,result)
        print(txt.group(0).replace('>','').replace('</','')+"("+href.group(0).replace('"','').replace('href=','')+")")
        count=count+1
    print('共' + str(count) + '篇文章')


def reqs(urllist):
    count=0
    reText=r'<a class="post-title-link" href=".*?" itemprop="url">.*?</a>'
    reResult=[]
    for url in urllist:
        response = requests.get(url)
        response.encoding = "utf-8"
        html=response.text
        reResult.extend(re.findall(reText,html))
    for result in reResult:
        reHref=r'href=".*?"'
        retxt=r'>.*?</'
        href=re.search(reHref,result)
        txt=re.search(retxt,result)
        print(txt.group(0).replace('>','').replace('</','')+"("+href.group(0).replace('"','').replace('href=','')+")")
        count=count+1
    print('共' + str(count) + '篇文章')


if __name__=='__main__':
    print('正在获取要爬取的URL……')
    urllist=getUrl()
    print('用beautifulsoup、requests、urllib3种爬虫爬取blog的所有文章的标题和链接')
    print('bs爬取：')
    start = time.time()
    bs(urllist)
    end = time.time()
    print('爬取用时'+str(end-start))
    print('urllib爬取：')
    start = time.time()
    ul(urllist)
    end = time.time()
    print('爬取用时'+str(end-start))
    print('requests爬取：')
    start = time.time()
    reqs(urllist)
    end = time.time()
    print('爬取用时'+str(end-start))

```
爬虫的功能很简单，就是分别用3种方法爬取我的网站的所有文章的标题和链接，输出如下：
```
正在获取要爬取的URL……
用beautifulsoup、requests、urllib3种爬虫爬取blog的所有文章的标题和链接
bs爬取：
游戏人生ZERO(/2018/03/03/NGNFZERO/)
iOS11.2.5崩溃bug(/2018/02/16/ios11-2-5-crush-bug/)
培根密码栅栏密码(/2018/02/15/bacon-fence/)
AES学习(/2018/02/13/aes-study/)
こどく(/2018/02/05/kodoku/)
照片隐私泄露(/2018/01/29/photo-privacy-leaked/)
一言API调用(/2018/01/14/hitokoto/)
提问的艺术(/2018/01/10/how-to-ask/)
Meltdown 攻击(/2018/01/05/Meltdown/)
Python学习笔记(/2018/01/03/python-study/)
2018，よろしくお願いします。(/2018/01/01/welcome2018/)
新的博客新的开始(/2017/12/30/newstart/)
Github搭建Hexo博客(/2017/12/30/GithubHexo/)
共13篇文章
爬取用时1.4970629215240479
urllib爬取：
游戏人生ZERO(/2018/03/03/NGNFZERO/)
iOS11.2.5崩溃bug(/2018/02/16/ios11-2-5-crush-bug/)
培根密码栅栏密码(/2018/02/15/bacon-fence/)
AES学习(/2018/02/13/aes-study/)
こどく(/2018/02/05/kodoku/)
照片隐私泄露(/2018/01/29/photo-privacy-leaked/)
一言API调用(/2018/01/14/hitokoto/)
提问的艺术(/2018/01/10/how-to-ask/)
Meltdown 攻击(/2018/01/05/Meltdown/)
Python学习笔记(/2018/01/03/python-study/)
2018，よろしくお願いします。(/2018/01/01/welcome2018/)
新的博客新的开始(/2017/12/30/newstart/)
Github搭建Hexo博客(/2017/12/30/GithubHexo/)
共13篇文章
爬取用时1.3659710884094238
requests爬取：
游戏人生ZERO(/2018/03/03/NGNFZERO/)
iOS11.2.5崩溃bug(/2018/02/16/ios11-2-5-crush-bug/)
培根密码栅栏密码(/2018/02/15/bacon-fence/)
AES学习(/2018/02/13/aes-study/)
こどく(/2018/02/05/kodoku/)
照片隐私泄露(/2018/01/29/photo-privacy-leaked/)
一言API调用(/2018/01/14/hitokoto/)
提问的艺术(/2018/01/10/how-to-ask/)
Meltdown 攻击(/2018/01/05/Meltdown/)
Python学习笔记(/2018/01/03/python-study/)
2018，よろしくお願いします。(/2018/01/01/welcome2018/)
新的博客新的开始(/2017/12/30/newstart/)
Github搭建Hexo博客(/2017/12/30/GithubHexo/)
共13篇文章
爬取用时0.9186527729034424
```
很明显，beautifulsoup和urllib速度相差不大，而requests相对来说快一点。
他们的优缺点通过代码也可以看出来。urllib和requests需要编写正则表达式来匹配内容，beautifulsoup则不用，通过标签选择就可以快速简单的选择出想要的内容。
而urllib是Python的标准库，不需要安装第三方库，request则相对来说速度更快些。