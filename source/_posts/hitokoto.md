---
title: 一言API调用
date: 2018-01-14 16:04:45
tags: 技术
---
[一言](http://hitokoto.cn/)是一个非常棒的一句话服务
关于如何调用一言API到自己的博客中呢?首先可以看一下一言的API的内容：http://hitokoto.cn/api
这里提供一个简单的调用js
```javascript
window.onload=function () {
    var hitokoto = document.querySelector('.hitokoto');
    var from = document.querySelector('.from');
    update();
    function update() {
        gethi = new XMLHttpRequest();
        gethi.open("GET","https://sslapi.hitokoto.cn/?c=a");
        //这里选择类别，详见官方文档
        gethi.send();
        gethi.onreadystatechange = function () {
            if (gethi.readyState===4 && gethi.status===200) {
                var Hi = JSON.parse(gethi.responseText);
                hitokoto.innerHTML = Hi.hitokoto;
                from.innerHTML = "from: <b>" + Hi.from + "</b>"; //可自定义输出格式
            }
        }
    }
}
```
接下来可以直接在网页上通过hitokoto和from两个class调用了，例如：
```html
<div>
    <p class="hitokoto"></p>
    <p class="from"></p>
</div>
```
如何在Hexo中调用一言呢？
很简单。以我的主题next为例。在目录`\themes\next\source\js\src`下新建`hitokoto.js`把调用js写进去，在`\themes\next\layout\_layout.swig`的最下面的`</body>`前新增一行
```html
<script type="text/javascript" src="/js/src/hitokoto.js"></script>
```
以后在写文章时在md文件里直接写
```html
<div>
    <p class="hitokoto"></p>
    <p class="from"></p>
</div>
```
就可以了。效果如下：
<div>
    <p class="hitokoto"></p>
    <p class="from"></p>
</div>
当然，你可以改成任何你喜欢的样子。js代码可以改，html也可以。我感觉这样斜体也不错（参见[关于](/about/)）：
```html
<div>
    <i class="hitokoto"></i><i class="from"></i>
</div>
```
值得注意的是，这种调用方法只能同时调用一次。如有需要可以自己改一下代码。
参考文章：[一言Hitokoto API 调用指南](https://2heng.xin/2017/08/12/hitokoto)