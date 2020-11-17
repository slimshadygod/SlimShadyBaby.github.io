---
title: python-类
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-14 12:28:33
password:
summary: python类详解
tags:
- python
categories:
- python
---

## 类的初始化

### __init__

python类初始化默认会调用`__init__`方法

``` python
class Car(object):
    def __init__(self, model, year):
        self.model = model
        self.year = year
```

### 权限控制

1. 默认情况下,`__init__`中的属性是可以被修改的
2. python中让属性变成`private`的方法是在属性前加两个下划线
3. 使用装饰器给类提供set和get的方法

``` python
class Car(object):

    def __init__(self, model, year) -> None:
        self.__model = model
        self.__year = year

    @property
    def model(self):
        return self.__model

    @model.setter
    def model(self, model):
        self.__model = model

    @property
    def year(self):
        return self.__year

    @year.setter
    def year(self, year):
        self.__year = year


if __name__ == "__main__":
    car = Car("audi", 3)
    print(f"the car model is {car.model}, year is {car.year}")
    print(f"the car model is {car.__model}, year is {car.__year}")

>>>
// 成功获取属性
the car model is audi, year is 3
// 不能访问私有方法
Traceback (most recent call last):
  File "e:\Code\python-study\001_basic_data_type.py", line 35, in <module>
    print(f"the car model is {car.__model}, year is {car.__year}")
AttributeError: 'Car' object has no attribute '__model'

if __name__ == "__main__":
    car = Car("audi", 3)
    car.model = "yadi" // 实际调用的是set方法
    car.year = 4 // 获取的是property
    print(f"the car model is {car.model}, year is {car.year}")

>>>
the car model is yadi, year is 4
```

## 类的继承

### 类的属性

1. 私有的`privite`: 使用两个下划线表示 `self.__year`
该属性不能被子类和实例对象方法
2. 受保护的`protected`: 使用一个下划线表示 `self._year`
该属性可以被子类访问,但不能被实例所访问
3. 公共的`public`: 没有下换线开头的 `self.year`
该属性可以被子类及实例对象访问

### 继承父类的构造方法

``` python
class Car(object):
    def __init__(self, make: str, model: str, year: str) -> None:
        self.__make = make
        self.__model = model
        self.__year = year

    @property
    def make(self):
        return self.__make

    @property
    def model(self):
        return self.__model

    @property
    def year(self):
        return self.__year


class ElectricCar(Car):

    def __init__(self, make: str, model: str, year: str, battery_size: int) -> None:
        super().__init__(make, model, year)
        self.battery_size = battery_size


if __name__ == "__main__":
    electricCar = ElectricCar("tesla", "model s", "2020", 75)
    print(f"the electric car desined by {electricCar.make}, model is {electricCar.model}, created by {electricCar.year}, battery_size is {electricCar.battery_size}")

>>>
the electric car desined by tesla, model is model s, created by 2020, battery_size is 75
```

### 重写父类的方法

给父类添加一个`to_string`的方法

``` python
class Car(object):
    // 单个下划线表示属性是protected的
    def __init__(self, make, model, year) -> None:
        self._make = make
        self._model = model
        self._year = year

    def to_string(self):
        print(f"{self._make}, {self._model}, {self._year}")

>>> electricCar.to_string()
tesla, model s, 2020
```

给子类添加一个同样的方法

``` python
class ElectricCar(Car):

    def __init__(self, make: str, model: str, year: str, battery_size: int) -> None:
        super().__init__(make, model, year)
        self.battery_size = battery_size

    def to_string(self):
        print(f"{self._make}, {self._model}, {self._year}, {self.battery_size}")


if __name__ == "__main__":
    electricCar = ElectricCar("tesla", "model s", "2020", 75)
    electricCar.to_string()

>>>
tesla, model s, 2020, 75
```
