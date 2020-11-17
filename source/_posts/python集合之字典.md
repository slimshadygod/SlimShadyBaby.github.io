---
title: python集合之字典
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-14 08:53:35
password:
summary: python字典的基本操作
tags:
- python
- 集合
categories:
- python
---

## 字典创建

1. 使用花括号{}表示创建一个空字典

2. 也可以直接在创建字典是赋一个默认值

    ``` python
    alien = {}

    alien = {"color": "green"}
    ```

## 字典的访问和修改

1. 访问的方式有两种, alien["color"], alien.get("color")

2. 推荐使用get方法,当可以不存在时,直接访问键值会产生异常,get方法可以返回默认值

    ``` python
    >>> alien = {"color": "green"}
    >>> alien["a"]  // 产生异常
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    KeyError: 'a'
    >>> alien.get("a") // 默认返回None
    >>> print(alien.get("a"))
    None
    >>> print(alien.get("a","this")) // get的第二个参数可以给返回值赋一个默认值
    this
    ```

## 字典的遍历

``` python
遍历键值
user = {"name": "efermi", "age": 16, "sex": "man"}
for key in user.keys():
    print(key)
>>>
name
age
sex

遍历排序过后的键值
user = {"name": "efermi", "age": 16, "sex": "man"}
for key in sorted(user.keys()):
    print(key)
>>>
age
name
sex

遍历value
user = {"name": "efermi", "age": 16, "sex": "man"}
for key in user.values():
    print(key)
>>>
efermi
16
man

遍历key和value
user = {"name": "efermi", "age": 16, "sex": "man"}
for key, value in user.items():
    print(key, value)
>>>
name efermi
age 16
sex man
```
