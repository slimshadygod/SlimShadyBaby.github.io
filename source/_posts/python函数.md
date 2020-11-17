---
title: python函数
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-14 09:31:30
password:
summary: python函数
tags:
- python
- 函数
categories:
- python
---

## 函数的定义

1. 形参: 定义函数时的占位符,没有实际的值
2. 实参: 调用函数时传入的参数,是实际的值
3. 参数的顺序,从左往右,必选参数,默认参数,可变参数,关键字参数

### 必选参数

参数只有一个形参,没有默认值式,调用的时候必需全部指定,不然会报错

``` python
def describe_pet(animal_type, pet_name):
    print(f"I have a {animal_type}")
    print(f"My {animal_type}'s name is {pet_name.title()}")

>>> describe_pet("dog", "wangwang")
I have a dog
My dog's name is Wangwang
>>> describe_pet("dog")
Traceback (most recent call last):
  File "e:\Code\python-study\001_basic_data_type.py", line 17, in <module>
    describe_pet("dog")
TypeError: describe_pet() missing 1 required positional argument: 'pet_name'
```

### 默认参数--*可选参数*

可以看到当调用参数时,如果没有传入age的值,也能成功输出age

``` python
def describe_pet(animal_type, pet_name, age=10):
    print(f"I have a {animal_type}")
    print(f"My {animal_type}'s name is {pet_name.title()}, age is {age}")

>>> describe_pet("dog", "wangwang")
I have a dog
My dog's name is Wangwang, age is 10
```

### 可变参数

可变参数也是一种可选参数,区别是可变参数可以不指定具体的形参,把所有的实参封装成一个tuple,可以根据tuple的索引获得传入的参数, 需要注意参数的顺序及越界问题,一般很少用这种方式

``` python
def describe_pet(*args):
    if len(args) >= 3:
        print(f"I have a {args[0]}")
        print(f"My {args[0]}'s name is {args[1].title()}, age is {args[2]}")
    else:
        print("please input at least three agruments")

>>> describe_pet("dog", "wangwang", 30)
I have a dog
My dog's name is Wangwang, age is 30

>>> describe_pet("dog", "wangwang")
please input at least three agruments
```

### 关键字参数

关键字参数也是一种可选参数,只是入参形式必须使用dict类型,需要注意传入的key值如果和函数中定义的key不一致,不会报错,但是对应的代码不会生效,`实际开发中很容易出现问题`

``` python
def describe_pet(**args):
    animal_name = args.get("animal_name")
    pet_name = args.get("pet_name")
    age = args.get("age")
    print(f"I have a {animal_name}")
    print(f"My {animal_name}'s name is {pet_name.title()}, age is {age}")

>>> describe_pet(animal_name="dog", pet_name="wangwang", age=30)
I have a dog
My dog's name is Wangwang, age is 30
```
