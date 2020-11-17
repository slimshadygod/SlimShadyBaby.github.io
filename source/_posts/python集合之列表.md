---
title: python集合之列表
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-13 23:21:04
password:
summary: python 列表的基本操作
tags:
- python
categories:
- python
---

## 列表的声明

``` python
bicyles = ["trek", "cannondale", "redline", "specilized"]
names = []
```

## 列表的访问

列表的访问通过下标的形式,从零开始,越界会出现异常

``` python
>>> bicyles = ["trek", "cannondale", "redline", "specilized"]
>>> bicyles[0]
'trek'
>>> bicyles[4]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
>>> bicyles[3]
'specilized'
>>>
```

## 列表的添加、修改、和删除

1. 添加方法:append(末尾添加),insert(指定位置插入)
2. 修改:针对索引操作
3. 删除: del(没有索引时,删除整个列表), pop(没有索引时,弹出最后一个元素),remove是删除具体的值

  ``` python
  修改
  >>> bicyles[1] = "taxi"
  >>> bicyles
  ['trek', 'taxi', 'redline', 'specilized']
  添加
  >>> bicyles.append("honda")
  >>> bicyles
  ['trek', 'taxi', 'redline', 'specilized', 'honda']
  >>> bicyles.insert(0,"yamaha")
  >>> bicyles
  ['yamaha', 'trek', 'taxi', 'redline', 'specilized', 'honda']
  删除
  del 有下标删除一个元素,没有下标删除整个列表
  >>> del bicyles[0]
  >>> bicyles
  ['trek', 'taxi', 'redline', 'specilized', 'honda']
  pop 有下标删除对应的值,没有下标删除最后一个
  >>> bicyles.pop()
  'honda'
  >>> bicyles
  ['trek', 'taxi', 'redline', 'specilized']
  >>> bicyles.pop(3)
  'specilized'
  >>> bicyles
  ['trek', 'taxi', 'redline']
  remove 删除具体的值,如果有多个相同的值,只会删除第一个
  >>> bicyles.remove("trek")
  >>> bicyles
  ['taxi', 'redline', 'specilized', 'honda']
  ```

### del和pop的区别

del没有返回值,pop有返回值,可以赋值给其他变量使用

``` python
>>> bicyles = ['yamaha', 'trek', 'taxi', 'redline', 'specilized', 'honda']
>>> temp = del bicyles[0]
  File "<stdin>", line 1
    temp = del bicyles[0]
             ^
SyntaxError: invalid syntax
>>> temp = bicyles.pop(0)
>>> temp
'yamaha'
>>> bicyles
['trek', 'taxi', 'redline', 'specilized', 'honda']
```

## 列表的排序

1. sort()会默认按照字母顺序排序
2. sorted()不会修改原来的列表,只是给一个临时的排好序的列表
3. reverse(),反转列表
4. len表示列表的长度
5. list[-1]返回最后一个元素,当列表为空时会有异常

``` python
>>> bicyles = ['trek', 'taxi', 'redline', 'specilized', 'honda']
>>> sorted(bicyles)
['honda', 'redline', 'specilized', 'taxi', 'trek']
>>> bicyles
['trek', 'taxi', 'redline', 'specilized', 'honda']

>>> bicyles.sort()
>>> bicyles
['honda', 'redline', 'specilized', 'taxi', 'trek']

>>> bicyles
['honda', 'redline', 'specilized', 'taxi', 'trek']
>>> bicyles.reverse()
>>> bicyles
['trek', 'taxi', 'specilized', 'redline', 'honda']

>>> len(bicyles)
5
```

## 列表的遍历

### for循环

使用for循环进行列表的遍历

``` python
names = ["Bob", "Alan", "Jim"]
for name in names:
    print(name)
>>>
Bob
Alan
Jim
```

### 数值型列表

使用range()函数可以轻松的创建一个数值型列表

``` python
>>> numbers = list(range(1,5))
>>> numbers
[1, 2, 3, 4]

for value in range(1, 12, 2):
    print(value)
>>>
1
3
5
7
9
11
```

### 列表解析式

``` python
sequence = [x**2 for x in range(1, 5)]
print(sequence)
>>>
[1, 4, 9, 16]
```

### 列表的切片

1. 左闭右开
2. 冒号的一边没有值,默认到边界
3. 冒号的两边没有值,直接复制整个列表
4. 列表的反向下标从-1开始

``` python
>>> nums = [1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> nums[1:3]
[2, 3]
>>> nums[:3]
[1, 2, 3]
>>> nums[3:]
[4, 5, 6, 7, 8, 9]
>>> nums[:] // 复制列表
[1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> nums[4:-1] // 注意左闭右开
[5, 6, 7, 8]
```
