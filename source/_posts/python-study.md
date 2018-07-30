---
title: Python学习笔记
date: 2018-01-03 21:16:32
tags: [技术,python,学习笔记]
---
最后更新时间：March 3, 2018 10:15 PM
*该笔记记录了我学习Python时发现的一些容易踩坑的地方。*

## 笔记内容
1. max和min方法
2. 字典方法clear
3. 字典方法copy(deepcopy)
4. 链式赋值
5. Python的bool值
6. Python的各种作用域和命名空间
7. lambda表达式
8. Python类的私有
9. Python新式类和旧式类
10. 其它的一些小要点

## max()和min()
最近学习Python，看书看到max和min函数，写的时候突发奇想，如果对一个混合各种类型的序列执行max和min函数会怎样。我开始学习使用的是Python2，用PyCharm执行代码，结果让我一脸懵逼。
```python
list=['1','2','45',3,4]
print len(list)
print max(list)
print min(list)
```
![pycharm](https://ooo.0o0.ooo/2018/01/03/5a4cd8a578228.png)
输出竟然是
```
5
45
3
```
<!-- more -->
一脸懵逼，当时我想，难道Python可以识别字符串？但是为什么最小是3呢，不该是2么？
查了很久资料，网上大多数的max和min函数介绍都是拿一个统一的序列讲的，包括我的书也是，统一的序列当然不会有问题了。翻了很久，终于找到答案了：[python max和min函数 --百度知道](https://zhidao.baidu.com/question/1112721080815769179.html)。大概意思是，Python的这两个函数识别是比较，不一样的类型的结果不同。可以在Python2环境下执行下面的代码试试：
```python
print "1">1
print "1">2
```
结果都是True，所以我可以这样认为，在Python2中，字符串比数字大。
为什么说Python2呢，因为那个答案下的另一个回答我看到了:
>就不要对这样混杂不同类型的数据用max或min。。。。
>1.逻辑不通，2.出现的结果是非标准定义的、随版本实现的、不可控的输出.
>在python3中已经增加了对这种异常的处理，会弹出不可排序的异常
>TypeError: unorderable types: list() < int()
>所以不要去理解，把max([9, 8, 7, [6, 5, 4]])当成错误的、不安全的代码写法

那我们拿Python3执行同样的代码结果如何？
![python2](https://ooo.0o0.ooo/2018/01/03/5a4cd8a5bc10c.png)
![python3](https://ooo.0o0.ooo/2018/01/03/5a4cd8a5e5418.png)
结果很明确了，在Python2中这种写法是允许的，但这样会导致一些错误，所以在Python3中这种被定义为`TypeError: '<' not supported between instances of 'int' and 'str'`错误（由错误也能看出来函数靠比较得到最大最小）
```python
"1">1
```
python3执行这句语句，会产生`TypeError: '>' not supported between instances of 'str' and 'int'`异常
结论：**写Python的序列要好好写，不要各种类型混在一起，Python3会抛出异常。Python2虽然没事，但可能会产生一些错误**
*January 3, 2018 9:44 PM*

## 字典方法
### clear
在书上看到一个很有趣的例子,我改了一下，大致是这样的
```python
x={}
y=x
x['key']='value'
print(x)
print(y)
x={}
print(x)
print(y)
```
结果是
```
{'key': 'value'}
{'key': 'value'}
{}
{'key': 'value'}
```
那试一试另一种写法
```python
x={}
y=x
x['key']='value'
print(x)
print(y)
x.clear()
print(x)
print(y)
```
结果是
```
{'key': 'value'}
{'key': 'value'}
{}
{}
```
书上的解释是这样的(提炼出了最关键的部分，不是原文)`两种情况中，x和y最初对应同一个字典。将x关联到一个新字典，y还关联到原先的字典。如果想清空原字典中所有的元素必须使用clear方法`。问了一些大佬，解释也都是这样的，`x={}`就**将x的指向修改到了新字典，但y的指向不变**，还是原字典，原字典不变。要清除干净这些用clear方法就清除干净了。（**这里注意，防止以后失误**）
为了进一步确定，我们用id方法来看一下
```python
x={}
y=x
x['key']='value'
print(x)
print(y)
print(id(x))
print(id(y))
x={}
print(x)
print(y)
print(id(x))
print(id(y))
```
```
{'key': 'value'}
{'key': 'value'}
19362680
19362680
{}
{'key': 'value'}
29329608
19362680
```
当`x={}`后，x就指向了新的字典，y不变。

### copy
copy返回一个具有相同键值对的新字典（**浅复制**）
下面2个代码
```python
x={'username':'admin','machines':['foo','bar','baz']}
y=x.copy()
y['username']='mlh'
y['machines'].remove('bar')
print(x)
print(y)
```
运行结果：
```
{'username': 'admin', 'machines': ['foo', 'baz']}
{'username': 'mlh', 'machines': ['foo', 'baz']}
```
使用copy的模块deepcopy函数（**深复制**）
```python
from copy import deepcopy
x={'username':'admin','machines':['foo','bar','baz']}
y=deepcopy(x)
y['username']='mlh'
y['machines'].remove('bar')
print(x)
print(y)
```
运行结果：
```
{'username': 'admin', 'machines': ['foo', 'bar', 'baz']}
{'username': 'mlh', 'machines': ['foo', 'baz']}
```
书上的解释是`当副本中替换值的时候，原始字典不受影响，但是如果修改了某个值（原地修改而不是替换），原始字典也会改变，因为原来的值也存储在原字典中`，避免这种问题的方法之一就是使用**深复制**，复制其包含的所有值。
看了一些网上的解释，大致是这样的
> 在Python中对象的赋值其实就是对象的引用。当创建一个对象，把它赋值给另一个变量的时候，python并没有拷贝这个对象，只是拷贝了这个对象的引用而已。
> 浅拷贝：拷贝了最外围的对象本身，内部的元素都只是拷贝了一个引用而已。也就是，把对象复制一遍，但是该对象中引用的其他对象我不复制
> 深拷贝：外围和内部元素都进行了拷贝对象本身，而不是引用。也就是，把对象复制一遍，并且该对象中引用的其他对象我也复制。

> 深浅拷贝都是对源对象的复制，占用不同的内存空间。
> 不可变类型的对象，对于深浅拷贝毫无影响，最终的地址值和值都是相等的。
> 可变类型：
> 浅拷贝： 值相等，地址相等
> copy浅拷贝：值相等，地址不相等
> deepcopy深拷贝：值相等，地址不相等

那么这么看来，copy和deepcopy后的结果应该这么解释：
deepcopy是全部复制，自然修改y不会影响x。而copy后修改y的`username`改变了是因为`username`是外面的父对象的一部分，浅拷贝时一起复制了过来，但`machines`包含了一个列表，是子对象，只复制了一个引用，修改时就都修改了。

我根据书上的说法，发现替换y的`mechines`的值后copyx不变。
想了很久，想到一个解释：替换修改的是那一个部分的整体，属于父对象，而remove之类的原地修改却修改的是上面列表中的一部分，是子对象，所以就都变了。

我们看这么一段代码：
```python
x={'username':'admin','machines':['foo','bar','baz']}
y=x.copy()
y['username']='mlh'
y['machines'].remove('bar')
print(x)
print(id(x))
print [id(x[ele]) for ele in x]
print(y)
print(id(y))
print [id(y[ele1]) for ele1 in y]
```
结果是
```
{'username': 'admin', 'machines': ['foo', 'baz']}
24087272
[24118816L, 24049608L]
{'username': 'mlh', 'machines': ['foo', 'baz']}
20746856
[24122352L, 24049608L]
```
copy对machines只复制了一个引用。
又分别测试了一下copy和deepcopy各部分的id
```
# copy
[31142096L, 31061960L]
[31142096L, 31061960L]
# deepcopy
[30815056L, 30734280L]
[30815056L, 30749512L]
```
在替换前，应该是为了节省空间，除了deepcopy对子对象外，都不进行新的内存分配。

## 链式赋值
Python有一个特点就是链式赋值，比如`x=y='willv'`这种语句。
链式赋值`x=y='willv'`本质上和下面这个代码是一样的
```python
x='willv'
y=x
```
但要注意，他们和这段代码不一样
```python
x='willv'
y='willv'
```
我们可以看这么一段代码就知道了
```python
x=y=['willv']
print(id(x))
print(id(y))
w=[1,2,3]
z=[1,2,3]
print(id(w))
print(id(z))
```
```
26645960
26645960
26646600
26683336
```

## Python的bool值
除了标准的True和False，所有类型的数字0（浮点、长整型等类型）、空序列（字符串、元组、列表）、空字典都为假（False），其它的一切都为真（True）。标准bool值False为0，True为1。
![bool](https://ooo.0o0.ooo/2018/02/28/5a96c73b05497.png)

## Python的作用域和命名空间
*March 13, 2018 10:22 PM*

## Lambda表达式

## Python类的私有

## Python新式类和旧式类
`__metaclass__=type`

## 生成器

## 其它
1. 两个变量同时引用一个列表，他们的确是同时引用同一个列表（同字典函数clear处）。**可以用`n=names[:]`复制一个列表以避免。**（注意函数使用时忽视这点导致**实参被修改**（也可利用以实现其它语言的引用功能,如C#的ref和C++的*或&））
![other1](https://ooo.0o0.ooo/2018/03/02/5a996405e0da8.png)

2. vars()locals()globals()等函数还不太清楚 super()超类