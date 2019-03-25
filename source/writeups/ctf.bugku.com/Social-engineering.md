---
title: bugku-社工
date: 2018-01-21 22:51:38
comments: true
---
## 密码

> 姓名：张三
> 生日；19970315
>
> KEY格式KEY{xxxxxxxxxx}

弱口令，直接猜就行

`KEY{zs19970315} `

## 信息查找

> 社会工程学基础题目 信息查找
>
> 听说bugku.cn 在今日头条上能找到？？
>
> 提示：flag为群号码
>
> 格式KEY{xxxxxxxxxxx}

使用搜索语法`bugku.cn site:toutiao.com`去谷歌搜就可以搜到一条头条的新闻。

![s2.png](https://i.loli.net/2019/03/24/5c97278a7a486.png)

里面就有群号，

`KEY{462713425} `

## 简单个人信息收集

给了一个压缩包，但是加密了。

![s2.png](https://i.loli.net/2019/03/24/5c9728837ecf5.png)

尝试压缩文件伪加密。（安利我的项目Crypto，其中有压缩文件伪加密的加解密类[CryptoZip](https://github.com/WWILLV/Crypto#cryptozip))

![s2.png](https://i.loli.net/2019/03/24/5c972a26dccba.png)

解密打开txt，内容为

```
在哈尔滨市阿城区胜利街六委十三组	有个叫杜甫的你能把他的手机号找到吗？

flag格式  flag{手机号}
```

查社工库，这就需要自己平时积累的社工库了。搜索社工库，在某流传较广数据里搜到了手机号`15206164164`

`flag{15206164164}`

## 社工进阶

> name:孤长离
>
> 提示：弱口令

这个信息很少。我们尝试百度一下`孤长离`看到一个百度个人主页，查看内容，应该这里就是突破口。

他在贴吧发了一个贴[听说最近出CTF了？？](https://tieba.baidu.com/p/4911421204)

```
我的邮箱 bkctftest@163.com 嘿嘿 给我发点flag吧


我手里也有flag哟要不要交换一下
```

得到邮箱`bkctftest@163.com`并获得了邮箱里有flag的暗示。

题目提示弱口令，尝试弱口令登录邮箱。密码是`a123456 `(网易验证略烦)

在已发送里找到flag

`KEY{sg1H78Si9C0s99Q} `

## 王晓明的日记

> 晓明建了一个私人日记本 <http://120.24.86.145:8002/xiaoming/>
> 通过社工我们找到了他的信息
> 姓名：王晓明
> QQ：1221224649
> 生日：1998.10.11
> 用户名：adair
> 手机号：1991881231
> 我们通过xx库找到了历史密码
> xm1998.
>
> 以上信息为题目虚拟，提示：多用bugku在线工具

bugku有一个在线社工字典生成。加上Burp爆破，密码是`ADAIR321321. `,得到flag

`flag{bugku-shegong_xmq} `

## 简单的社工尝试

> 这个狗就是我画的，而且当了头像 这题提示的其实很明显了 想想吧 

![1.png](https://i.loli.net/2019/03/24/5c9724565afc2.png)

既然说狗是画的，说明这张图片应该是唯一的，原图做了头像。

去谷歌识图搜一下这张图。

发现了一个Github页面，使用了这个头像[Github指路](https://github.com/bugku )

![s2.png](https://i.loli.net/2019/03/24/5c97255acba8d.png)

这里有一个微博地址[https://weibo.com/bugku](https://weibo.com/bugku)，去这个微博可以看到这么一条

![s2.png](https://i.loli.net/2019/03/24/5c9725ea850df.png)

去图中地址就可以看到Flag

`flag{BUku_open_shgcx1} `

