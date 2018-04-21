---
title: 培根密码栅栏密码
date: 2018-02-15 14:30:01
tags: [技术,安全,密码学,学习笔记,Crypto]
---
接上一篇的AES，在写[Crypto类库](https://github.com/WWILLV/Crypto)时也写了培根密码和栅栏密码，这里也写一下这两中加密算法。

## 培根密码
培根密码，又名倍康尼密码（英语：Bacon's cipher）是由法兰西斯·培根发明的一种隐写术。[维基百科]

### 原理
加密时，明文中的每个字母都会转换成一组五个英文字母。其转换依靠下表：
```
a   AAAAA   g     AABBA   n    ABBAA   t     BAABA
b   AAAAB   h     AABBB   o    ABBAB   u-v   BAABB
c   AAABA   i-j   ABAAA   p    ABBBA   w     BABAA
d   AAABB   k     ABAAB   q    ABBBB   x     BABAB
e   AABAA   l     ABABA   r    BAAAA   y     BABBA
f   AABAB   m     ABABB   s    BAAAB   z     BABBB
```
这只是一款最常用的加密表，有另外一款将每种字母配以不同的字母组予以转换，即I与J、U与V皆有不同编号。

加密者需使用两种不同字体，分别代表A和B。准备好一篇包含相同AB字数的假信息后，按照密文格式化假信息，即依密文中每个字母是A还是B分别套用两种字体。

解密时，将上述方法倒转。所有字体一转回A，字体二转回B，以后再按上表拼回字母。

法兰西斯·培根另外准备了一种方法，其将大小写分别看作A与B，可用于无法使用不同字体的场合（例如只能处理纯文本时）。但这样比起字体不同更容易被看出来，而且和语言对大小写的要求也不太兼容。

培根密码本质上是将二进制信息通过样式的区别，加在了正常书写之上。培根密码所包含的信息可以和用于承载其的文章完全无关。
<!-- more -->
### 代码实现
代码这里只写加密算法了，解密也类似于摩斯密码，具体可以去Github看我的[Crypto类库](https://github.com/WWILLV/Crypto)，使用时直接引用就行了。
```cs
static private string[] baconArray = { "AAAAA", "AAAAB", "AAABA", "AAABB", "AABAA", "AABAB",
        "AABBA","AABBB","ABAAA","ABAAA","ABAAB","ABABA","ABABB","ABBAA","ABBAB","ABBBA","ABBBB",
        "BAAAA","BAAAB","BAABA","BAABB","BAABB","BABAA","BABAB","BABBA","BABBB"};

/// <summary>
/// 培根加密
/// </summary>
/// <param name="str">明文</param>
/// <returns>密文</returns>
static public string baconEncrypt(string str)
{
    str = str.ToUpper();
    string result = "";
    for (int i = 0; i < str.Length; i++)
    {
        char ch = str[i];
        if (ch >= 'A' && ch <= 'Z')
        {
            result += baconArray[ch - 'A'];
        }
        else
            throw new Exception("输入有误");
    }
    return result;
}

```

### 结果
```
请输入要加解密的字符串：willv
培根加密结果为：BABAAABAAAABABAABABABAABB
请输入要加解密的字符串：BABAAABAAAABABAABABABAABB
培根加密结果为：WI(J)LLU(V)
```

## 栅栏密码
栅栏密码，就是把要加密的明文分成N个一组，然后把每组的第1个字连起来，形成一段无规律的话。 不过栅栏密码本身有一个潜规则，就是组成栅栏的字母一般不会太多。（一般不超过30个，也就是一、两句话）

### 加解密实例
[百度百科]
一般比较常见的是2栏的栅栏密码。
比如明文：THERE IS A CIPHER
去掉空格后变为：THEREISACIPHER
两个一组，得到：TH ER EI SA CI PH ER
先取出第一个字母：TEESCPE
再取出第二个字母：HRIAIHR
连在一起就是：TEESCPEHRIAIHR
还原为所需密码。
而解密的时候，我们先把密文从中间分开，变为两行：
T E E S C P E
H R I A I H R
再按上下上下的顺序组合起来：
THEREISACIPHER
分出空格，就可以得到原文了：
THERE IS A CIPHER
不是所有密码都分为两栏，比如：
明文：THERE IS A CIPHER
七个一组：THEREIS ACIPHER
抽取字母：TA HC EI RP EH IE SR
组合得到密码：TAHCEIRPEHIESR
那么这时候就无法再按照2栏的方法来解了...
1分析解码这样，我们可以通过分析密码的字母数来解出密码...
比如：TAHCEIRPEHIESR
一共有14个字母，可能是2栏或者7栏...
尝试2栏...失败
尝试7栏...成功

### 代码实现
栅栏密码的代码实现也比较简单。我还另外写了一个获取栅栏数的函数。
```cs
str = str.Replace(" ", "");
int length = str.Length;
int[] result;
Stack<int> s = new Stack<int> { };
//分解因数
for (int i = 2; i < System.Math.Sqrt(length); i++)  //博扬大佬说到开根号就行了
{
    if (length%i==0)
    {
        s.Push(i);
    }
}
result = s.Reverse().ToArray();
return result;
```

以栅栏密码加密为例（我在类库Crypto中加入了移除空格，如果需要空格的话需要重编译一下删除替换代码）
```cs
/// <summary>
/// 栅栏密码加密
/// </summary>
/// <param name="str">明文</param>
/// <param name="num">栅栏数</param>
/// <returns></returns>
static public string fenceEncrypt(string str,int num)
{
    /*
     * 明文：THERE IS A CIPHER
     * 七个一组：THEREIS ACIPHER
     * 抽取字母：TA HC EI RP EH IE SR
     * 组合得到密码：TAHCEIRPEHIESR
     */
    str = str.Replace(" ", "");
    if (str.Length%num!=0)
    {
        throw new Exception("栅栏数错误");
    }
    int cp = str.Length / num;  //可分的组数
    string[] temp = new string[cp]; //保存分组
    for (int i = 0; i < cp; i++)    //为分组复制
    {
        for (int j = 0; j < num; j++)
        {
            temp[i] += str[i * num + j];
        }
    }
    string result = "";
    //抽取字母
    for (int i = 0; i < num; i++)
    {
        for (int j = 0; j < cp; j++)
        {
            result += temp[j][i];
        }
    }
    return result;
}
```
栅栏密码的解密很简单，比如长度35的明文栅栏数5加密后，对密文进行以栅栏数为7再加密一次就可以解密了，数学证明很简单在这里就不写了。

### 结果
以实例中的字符串`THERE IS A CIPHER`为例
```
请输入要加解密的字符串：THERE IS A CIPHER
栅栏加密结果为：TEESCPEHRIAIHR
请输入要加解密的字符串：TEESCPEHRIAIHR
栅栏解密结果为：THEREISACIPHER
```