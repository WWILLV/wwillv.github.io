---
title: you-get-ffmpeg（也许现在需要加一个youtube-dl了）
date: 2018-01-21 22:03:48
comments: false
---

本工具用于Windows下，结合you-get、youtube-dl和FFMPEG组成的工具。

安利[BiliBiliPlayerInfo](https://github.com/WWILLV/BiliBiliPlayerInfo)，一个采集一些BiliBili视频播放页的数据的类库+工具（可采集aid，cid等数据并生成H5播放页）

![you-get-ffmpeg](https://i.loli.net/2018/01/21/5a64a6de1e83a.png)

## you-get、youtube-dl和FFMPEG
[you-get(github)](https://github.com/soimort/you-get) [you-get.org](https://you-get.org/)
>You-Get is a tiny command-line utility to download media contents (videos, audios, images) from the Web, in case there is no other handy way to do it.

[FFMPEG(github)](https://github.com/FFmpeg/FFmpeg) [ffmpeg.org](http://ffmpeg.org/)
>FFmpeg is a collection of libraries and tools to process multimedia content such as audio, video, subtitles and related metadata.

[youtube-dl(github)](https://github.com/rg3/youtube-dl) [rg3.github.io/youtube-dl/](http://rg3.github.io/youtube-dl/)
>Command-line program to download videos from YouTube.com and other video sites.

安利一个不错的cmd代替工具，[cmder](http://cmder.net/)
## 下载
[点此到Github找下载](https://github.com/WWILLV/you-get-ffmpeg#%E4%B8%8B%E8%BD%BD)

** 我的you-get.exe和youtube-dl.exe来自官方项目exe，release出更新exe我会尽快更新，如果等不及可以删除掉you-get.exe和youtube-dl.exe，使用pip3 install you-get安装或pip3 install --upgrade you-get更新you-get（均需要Python3环境），youtube-dl可以用-U参数 **

## 使用方法
* you-get、ffmpeg、ffplay、ffprobe、youtube-dl

>这些文件来自于you-get、youtube-dl和FFMPEG项目，具体使用方法可以使用-h参数（或--help）查看

>常用命令：

>"you-get 播放地址"直接下载文件

>"you-get -u 播放地址"返回真实地址

>"you-get -p 播放器地址 播放地址"使用播放器播放

>"ffmpeg -i [输入文件名] [参数选项] -f [格式] [输出文件]"ffmpeg转换格式

>"ffplay 媒体地址"ffplay播放文件（播放中快捷键这里不介绍了，具体可以看[这里](http://www.tuicool.com/articles/jiyu6b)）

>"ffprobe 媒体地址"获取媒体的详细信息

>youtube-dl太强大了，参数太多就不列了，直接后面加地址下载就行了

* clean

>清除download文件夹中所有文件

* home

>返回运行目录（download）

* play

>相当于"you-get -p ffplay 播放地址"命令，解析出地址后直接调用ffplay在线播放

* show

>在资源管理器中打开当前文件夹

* support

>查看you-get支持的网站地址

* v2m

>自动使用FFMPEG进行Video到MP3的转换

* ls

>列目录（来自[git](https://github.com/git/git)）

* xmp

>如果电脑上安装了迅雷影音，调用迅雷影音播放

* storm

>如果电脑上安装了暴风影音，调用暴风影音播放

* potplayer

>如果电脑上安装了potplayer，调用potplayer播放

* git

>查看本工具的git

* update

>跳转到you-get的git界面

* edit

>记事本编辑本工具文件，如：编辑more.bat为edit command\more.bat

* vip

>调用VIP视频解析（接口更新全看缘分，我如果没有看VIP视频的需求可能就不更新了）
>根据you-get项目的声明，you-get不解析VIP视频（即使加载cookie）

* more

>查看更多命令

## 注意
***更换设备需要修改的文件***

修改方法：右键.bat程序，编辑，保存即可

***以下为一些主要命令的修改（非必须改动或使用，但建议修改）***

command目录下clean.bat中download文件夹路径

command目录下home.bat中的路径

***以下为调用you-get -p的播放器快速调用的修改（非必须改动或使用）***

command目录下xmp.bat中迅雷影音的XMP.exe(如果存在)的路径

command目录下storm.bat中暴风影音的StormPlayer.exe(如果存在)的路径

command目录下potplayer.bat中potplayer的PotPlayerMini64.exe(如果存在)的路径

（注意更换盘符）

（也可以使用Chrome、Firefox等浏览器播放，但浏览器支持在线播放的文件类型太少，故未添加）

**可以使用@set download=..\download来代替，但这样一旦跨路径就可能引起不可预料的后果

## LICENSE
[ffmpeg](https://github.com/FFmpeg/FFmpeg#license)

[you-get](https://github.com/soimort/you-get/blob/develop/LICENSE.txt)

[youtube-dl](https://github.com/rg3/youtube-dl/blob/master/LICENSE)

## TODO
- [ ] GUI界面（这个其实没什么动力写）