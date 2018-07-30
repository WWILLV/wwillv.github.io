---
title: 游戏人生ZERO
date: 2018-03-03 22:15:28
tags:
---
游戏人生剧场版ZERO
（这个主要是dplayer和onedrive分享视频链接的测试，该视频随时会消失）<!--more-->
{% dplayer "url=https://onedrive.gimhoy.com/sharepoint/aHR0cHM6Ly9jb255YS1teS5zaGFyZXBvaW50LmNvbS86djovZy9wZXJzb25hbC93aWxsdl9tYWlsX2ZjemJsX3ZpcC9FVmM3czhibG5XUkV0aGNhenR2LU5Hc0JkVHg5ZE4tTmJraDlUVV80andHRmdRP2U9T0VjMW9T.mp4" "pic=https://onedrive.gimhoy.com/sharepoint/aHR0cHM6Ly9jb255YS1teS5zaGFyZXBvaW50LmNvbS86aTovZy9wZXJzb25hbC93aWxsdl9tYWlsX2ZjemJsX3ZpcC9FVEc4UzR6bXlDVktnZlBhang3OGQzc0JWMGtsdWcwMDVqZzV5RjRIbDBEVUhRP2U9MDlYenFL.png" "api=https://api.prprpr.me/dplayer/" "theme=#FADFA3" "autoplay=false" "token=tokendemo" "addition=https://api.prprpr.me/dplayer/v2/bilibili?cid=32515942" %}

```
npm install hexo-tag-dplayer --save
```
[https://github.com/MoePlayer/hexo-tag-dplayer](https://github.com/MoePlayer/hexo-tag-dplayer)
```
{% dplayer "url=https://onedrive.gimhoy.com/sharepoint/aHR0cHM6Ly9jb255YS1teS5zaGFyZXBvaW50LmNvbS86djovZy9wZXJzb25hbC93aWxsdl9tYWlsX2ZjemJsX3ZpcC9FVmM3czhibG5XUkV0aGNhenR2LU5Hc0JkVHg5ZE4tTmJraDlUVV80andHRmdRP2U9T0VjMW9T.mp4" "pic=https://onedrive.gimhoy.com/sharepoint/aHR0cHM6Ly9jb255YS1teS5zaGFyZXBvaW50LmNvbS86aTovZy9wZXJzb25hbC93aWxsdl9tYWlsX2ZjemJsX3ZpcC9FVEc4UzR6bXlDVktnZlBhang3OGQzc0JWMGtsdWcwMDVqZzV5RjRIbDBEVUhRP2U9MDlYenFL.png" "api=https://api.prprpr.me/dplayer/" "theme=#FADFA3" "autoplay=false" "token=tokendemo" "addition=https://api.prprpr.me/dplayer/v2/bilibili?cid=32515942" %}
```

开始使用调用时发现onedrive要调用直链必须要先访问一下资源（我这里通过iframe访问），然后刷新一下，直接链接才会提供下载流量以调用下载。（很麻烦）
过段时间发现了有工具可以直接提供onedrive直链调用[OneDrive直链获取工具](https://onedrive.gimhoy.com/)
B站原视频删掉了，但通过cid依旧可以调用原来的弹幕库。