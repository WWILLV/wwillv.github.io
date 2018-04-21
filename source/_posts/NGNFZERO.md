---
title: 游戏人生ZERO
date: 2018-03-03 22:15:28
tags:
---
游戏人生剧场版ZERO
（这个主要是dplayer和onedrive分享视频链接的测试，该视频随时会消失）<!--more-->
[**此处有2个1像素的iframe**]<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=1 height=1 src="https://conya-my.sharepoint.com/:v:/g/personal/willv_mail_fczbl_vip/EVc7s8blnWREthcaztv-NGsBoplSvB7Gd5xzkmtrTEqqFg"></iframe><iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=1 height=1 src="https://conya-my.sharepoint.com/:i:/g/personal/willv_mail_fczbl_vip/ETG8S4zmyCVKgfPajx78d3sBonJ6kyCxXRr0mn3_W7dTpA"></iframe><b style="color:red">看不到视频（时间显示为0或者没有封面什么的）刷新一下该页面！</b>
{% dplayer "url=https://conya-my.sharepoint.com/personal/willv_mail_fczbl_vip/_layouts/15/download.aspx?SourceUrl=%2Fpersonal%2Fwillv%5Fmail%5Ffczbl%5Fvip%2FDocuments%2Fwillv%2Fvideo%2FNoGameNoLifeZERO%2Emp4" "pic=https://conya-my.sharepoint.com/personal/willv_mail_fczbl_vip/_layouts/15/download.aspx?SourceUrl=%2Fpersonal%2Fwillv%5Fmail%5Ffczbl%5Fvip%2FDocuments%2Fwillv%2Fvideo%2FNoGameNoLifeZERO%2Epng" "api=https://api.prprpr.me/dplayer/" "theme=#FADFA3" "autoplay=false" "token=tokendemo" "addition=https://api.prprpr.me/dplayer/v2/bilibili?cid=32515942" %}

```
npm install hexo-tag-dplayer --save
```
```
{% dplayer "url=https://conya-my.sharepoint.com/personal/willv_mail_fczbl_vip/_layouts/15/download.aspx?SourceUrl=%2Fpersonal%2Fwillv%5Fmail%5Ffczbl%5Fvip%2FDocuments%2Fwillv%2Fvideo%2FNoGameNoLifeZERO%2Emp4" "pic=https://conya-my.sharepoint.com/personal/willv_mail_fczbl_vip/_layouts/15/download.aspx?SourceUrl=%2Fpersonal%2Fwillv%5Fmail%5Ffczbl%5Fvip%2FDocuments%2Fwillv%2Fvideo%2FNoGameNoLifeZERO%2Epng" "api=https://api.prprpr.me/dplayer/" "theme=#FADFA3" "autoplay=false" "token=tokendemo" "addition=https://api.prprpr.me/dplayer/v2/bilibili?cid=32515942" %}
```

经过一个多小时和一群人的测试发现，onedrive要调用直链必须要先访问一下资源（我这里通过iframe访问），然后刷新一下，直接链接才会提供下载流量以调用下载。（很麻烦）
严格来说这样算盗链吧，以后视频还是要么直接丢OneDrive链接，要么找到一个比较好的存储放视频，这样用实在是太麻烦了。
B站原视频删掉了，但通过cid依旧可以调用原来的弹幕库。