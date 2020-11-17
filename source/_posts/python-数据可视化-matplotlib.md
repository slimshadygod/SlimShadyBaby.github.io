---
title: python-数据可视化-matplotlib
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-14 16:32:27
password:
summary: python数据可视化
tags:
- matplotib
categories:
- python
---

## 绘制简单的图表

### 安装

`` pip install --user matplotlib`

### 绘制一个折线图

``` python
import matplotlib.pyplot as plt


if __name__ == "__main__":
    squares = [1, 4, 9, 16, 25] // 存放需要绘制的点
    fig, ax = plt.subplots() // fig表示整张图片, ax表示图片中的图表
    ax.plot(squares)
    plt.show()
```
