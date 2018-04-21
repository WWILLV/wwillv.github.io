---
title: 照片隐私泄露
date: 2018-01-29 10:03:22
tags: [技术,安全,隐私泄露]
---
起因是我从某RBQ群里保存了一张妹子的大腿图，晚上在床上看相册时发现iOS自动整理的地点相簿里多了那一张大腿图，点进去我看到了非常详细的信息，包括妹子的地址。虽然我知道这是怎么回事，我自己拍的照片也无所谓，但是其他人不一样。所以我萌生了写一篇文章来说一下照片的隐私泄露的问题。想看照片信息隐私泄露直接跳到第3节就行了，前2节是关于EXIF的介绍。

## 什么是EXIF
（百度百科）Exif是一种图像文件格式，它的数据存储与JPEG格式是完全相同的。实际上Exif格式就是在JPEG格式头部插入了数码照片的信息，包括拍摄时的光圈、快门、白平衡、ISO、焦距、日期时间等各种和拍摄条件以及相机品牌、型号、色彩编码、拍摄时录制的声音以及GPS全球定位系统数据、缩略图等。你可以利用任何可以查看JPEG文件的看图软件浏览Exif格式的照片，但并不是所有的图形程序都能处理Exif信息。
简而言之，对于一般人来说，EXIF存储了许多隐私信息，比如，你的位置……
<!--more-->

## EXIF标准格式
| 标签号 | Exif 定义名 | 中文定义名 | 备注 |
|--------|--------|--------|--------|
|0x010E|ImageDescription|图像描述|-|
|0x013B|Artist|作者|使用者的名字|
|0x010F|Make|生产商|相机生产厂家|
|0x0110|Model|型号|相机型号|
|0x0112|Orientation|方向|有的相机支持，有的不支持|
|0x011A|XResolution|水平方向分辨率|-|
|0x011B|YResolution|垂直方向分辨率|-|
|0x0128|ResolutionUnit|分辨率单位|-|
|0x0131|Software|软件|固件Firmware版本或编辑软件|
|0x0132|DateTime|日期和时间|照片最后的修改时间|
|0x0213|YCbCrPositioning|YCbCr定位|色度抽样方法|
|0x8769|ExifOffset|Exif子IFD偏移量|-|
|0x829A|ExposureTime|曝光时间|即快门速度|
|0x829D|FNumber|光圈系数|光圈的F值|
|0x8822|ExposureProgram|曝光程序|自动曝光、光圈优先、快门优先、M档等|
|0x8827|ISOSpeedRatings|ISO感光度|Exif 2.3 中更新为 “PhotographicSensitivity”|
|0x9000|ExifVersion|Exif 版本|参见“历史版本”一节|
|0x9003|DateTimeOriginal|拍摄时间|照片拍摄的时间|
|0x9004|DateTimeDigitized|数字化时间|照片被写入内存卡的时间|
|0x9204|ExposureBiasValue|曝光补偿|-|
|0x9205|MaxApertureValue|最大光圈|APEX为单位|
|0x9207|MeteringMode|测光模式|平均测光、中央重点测光、点测光等|
|0x9208|Lightsource|光源|一般记录白平衡设定|
|0x9209|Flash|闪光灯|记录闪光灯状态|
|0x920A|FocalLength|镜头焦距|镜头物理焦距|
|0x927C|MakerNote|厂商注释|参见“厂商注释”一节|
|0x9286|UserComment|用户注释|用户自定义数据|
|0xA000|FlashPixVersion|FlashPix版本|-|
|0xA001|ColorSpace|色彩空间|一般为sRGB|
|0xA002|ExifImageWidth|图像宽度|图像横向像素数|
|0xA003|ExifImageLength|图像高度|图像纵向像素数|
|0xA433|LensMake|镜头生产商|-|
|0xA434|LensModel|镜头型号|-|

## 照片EXIF信息隐私泄露
我拿我以前拍的2张照片为例
第一张是我在北大拍的未名湖，第二张是我在云南旅游拍的照片
![IMG_0317.JPG](https://ooo.0o0.ooo/2018/01/29/5a6e89f56fb40.jpg)
[![IMG_3437-1.PNG](https://ooo.0o0.ooo/2018/01/29/5a6e8a7d1c86a.png)](https://ooo.0o0.ooo/2018/01/29/5a6e8a7d1c86a.png)
![IMG_2817.JPG](https://ooo.0o0.ooo/2018/01/29/5a6e89f551d10.jpg)
[![IMG_3438-1.PNG](https://ooo.0o0.ooo/2018/01/29/5a6e8a7d25c56.png)](https://ooo.0o0.ooo/2018/01/29/5a6e8a7d25c56.png)
根本不需要什么专业软件，图片属性里位置写的很清楚，通过经纬度就可以直接定位（百度谷歌地图等也有API可通过经纬度直接定位）。iOS的照片也提供了定位功能（原意应该是方便用户旅行记录，但也方便了直接利用定位）
![IMG_3430.PNG](https://ooo.0o0.ooo/2018/01/29/5a6e8b6f91b5f.png)
（不要在意那个大腿，那个不是我的，是今天我要说的泄露隐私的）
![IMG_3437.PNG](https://ooo.0o0.ooo/2018/01/29/5a6e8b7214652.png)
![IMG_3438.PNG](https://ooo.0o0.ooo/2018/01/29/5a6e8b71df53d.png)
网上有非常详细的[EXIF查看器](https://exif.tuchong.com/)
我对我的上面2张照片进行了分析，截图就不放上来了。总之，你用的手机、拍照时间地点、模式、曝光、光圈、快门、焦距、视角、色彩等信息非常详细。这些信息对于专业摄影的人来说很重要，但是对于一般人来说就是属于隐私了。为什么这样说？我从微博、和某个RBQ群保存了3张图片，因为图片涉及其他人的隐私，这里就不放了，放一点处理过的信息：
![3.png](https://ooo.0o0.ooo/2018/01/29/5a6e8dd5b4ef7.png)
（那2个大腿都不是我的，那个风景照是我上面举例说过的，这里不说了）
（关键信息已打码）
![11.png](https://ooo.0o0.ooo/2018/01/29/5a6e8dd6521f3.png)
![12.png](https://ooo.0o0.ooo/2018/01/29/5a6e8dd5e081e.png)
![21.png](https://ooo.0o0.ooo/2018/01/29/5a6e8dd6488b1.png)
![22.png](https://ooo.0o0.ooo/2018/01/29/5a6e8dd5ef6f8.png)
![31.png](https://ooo.0o0.ooo/2018/01/29/5a6e8dd654552.png)
![32.png](https://ooo.0o0.ooo/2018/01/29/5a6e8dd5f10c6.png)
拍照地址非常详细，街道及地图已打码

## 防止EXIF泄露信息
如果自己用，不流传的图片，那无所谓。如果自己拍了照片要外传如何防止信息泄露呢？
首先，拍照前把相机的定位权限关闭掉。
其次，如果对照片清晰度没有要求，不要发原图。QQ微博等软件如果不勾选原图，都会对图片进行二次压缩，二次压缩后的图片信息会损失。
最后，如果以上2条都不行，那么可以在电脑上图片属性里清理掉这些信息，或者使用专门的软件清理掉敏感信息。