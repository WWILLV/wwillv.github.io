---
title: Python学习笔记
date: 2018-01-03 21:16:32
tags: [技术,python,学习笔记]
---
## max()和min()
最近学习Python，看书看到max和min函数，写的时候突发奇想，如果对一个混合各种类型的序列执行max和min函数会怎样。我开始学习使用的是Python2，用PyCharm执行代码，结果让我一脸懵逼。
```python
list=['1','2','45',3,4]
print len(list)
print max(list)
print min(list)
```
![pycharm](https://i.loli.net/2018/01/03/5a4cd8a578228.png)
输出竟然是
```
5
45
3
```
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
![python2](https://i.loli.net/2018/01/03/5a4cd8a5bc10c.png)
![python3](https://i.loli.net/2018/01/03/5a4cd8a5e5418.png)
结果很明确了，在Python2中这种写法是允许的，但这样会导致一些错误，所以在Python3中这种被定义为`TypeError: '<' not supported between instances of 'int' and 'str'`错误（由错误也能看出来函数靠比较得到最大最小）
```python
"1">1
```
python3执行这句语句，会产生`TypeError: '>' not supported between instances of 'str' and 'int'`异常
结论：**写Python的序列要好好写，不要各种类型混在一起，Python3会抛出异常。Python2虽然没事，但可能会产生一些错误**
*January 3, 2018 9:44 PM更新*