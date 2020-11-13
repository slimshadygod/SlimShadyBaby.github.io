---
title: python数据类型
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-13 22:53:47
password:
summary: python变量和基本数据类型
tags:
- python
categories:
- python
---

# 字符串

## 字符串的声明
``` python
strings = "This is a string" // 双引号
strings = 'This is a string' // 单引号
```

## 字符串的大小写
``` python
name = "Slim Shady"
print(name.upper())
print(name.lower())
```

## 字符串中使用变量
* 在字符串前面加上字母f,然后把变量放在花括号内,即可进行变量的替换
* f是format的缩写
``` python
name = "Slim Shady"
sex = "男"
print(f"name:{name}, sex:{sex}")
```
* 注意f的写法必须高于python3.6,python3.5及之前的写法是
``` python
full_name = "{} {}".format(first_name, last_name)
```

## 删除空白
``` python
>>> language = " python "
>>> language.rstrip()
' python'
>>> language.lstrip()
'python '
>>> language.strip()
'python'
```

# 数字

## 数字的基本运算
``` python
>>> 2+3
5
>>> 2-3
-1
>>> 2*3
6
>>> 2/3
0.6666666666666666
>>> 2/2
1.0
>>> 0.1 + 0.2
0.30000000000000004
>>> 100_000_000
100000000
```
1. 我们可以发现python中除法的结果都是浮点数,即使参与运算的都是整型
2. 浮点型小数的运算结果是不准确的,0.1 + 0.2 != 0.3
3. 数字可以用下划线来表示,更清晰易读（注意python版本>=3.6）

# 变量与常量
``` python
x, y, z = 0, 1, 3
MAX_NUM = 100000
```
1. 可以同时给多个变量进行赋值
2. 常量一般用全大写字母来表示

