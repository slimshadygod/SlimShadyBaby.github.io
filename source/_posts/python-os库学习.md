---
title: python-os库学习
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-14 14:32:19
password:
summary: python-os库的常见用法
tags:
- python
- os
categories:
- python
---

## 目录操作

### 常见命令

1. `pwd = os.getcwd()`: 获取当前运行程序的目录
2. `os.chdir(path)`: 切换目录
3. `dirs = os.mkdir(path, mode)`: 新建一个文件,mode默认=0777
4. `os.removedir(path)`: 递归删除文件夹及子文件
5. `os.remove(path)`: 删除文件,如果是文件夹会报错
6. `file_list = os.listdir(path)`: 查看文件夹下的文件及文件夹名称
7. `file = os.open(path, "w", encoding)`: 创建文件;  `file.close()`: 关闭文件

### 常见操作

``` python
1.获取当前文件路径
print(os.path.abspath(__file__))
e:\Code\python-study\003-os-study.py

2.获取当前文件的文件夹路径
print(os.path.dirname(__file__))
e:\Code\python-study

import os

print('***获取当前目录***')
print(os.getcwd())
print(os.path.abspath(os.path.dirname(__file__)))

***获取当前目录***
E:\Code\python-study
e:\Code\python-study

print('***获取上级目录***')
print(os.path.abspath(os.path.dirname(os.path.dirname(__file__))))
print(os.path.abspath(os.path.dirname(os.getcwd())))
print(os.path.abspath(os.path.join(os.getcwd(), "..")))

***获取上级目录***
e:\Code
E:\Code
E:\Code

print('***获取上上级目录***')
print(os.path.abspath(os.path.join(os.getcwd(), "../..")))

***获取上上级目录***
E:\
```

## 文件的读写

### 基本读写

``` python
1.读文件
with open("file_name.txt", "r", encoding="utf-8") as f:
        for line in f:
            print(line)

2.覆盖写文件
with open("file_name.txt", "w", encoding="utf-8") as f:
    file.write("abc\n")
    file.flush()

3.追加写
with open("file_name.txt", "a", encoding="utf-8") as f:
    file.write("abc\n")
    file.flush()
```

### 读取文件的异常处理

读取文件的时候,可能会出现文件找不到的问题

1. 判断文件是否存在
判断文件用`os.path.isfile(path)`,判断文件夹用`os.path.exists(path)`,存在返回true,不存在返回false

    ``` python
    file_path = "E:\\Code\\python-study\\001_basic_data_type1.py"
    if os.path.isfile(file_path):
        with open(file_path, "r", encoding="utf-8") as f:
            for line in f:
                print(line)
    else:
        print(f"file {file_path} not found.")

    >>>
    file E:\Code\python-study\001_basic_data_type1.py not found.
    ```

2. 使用try except

    ``` python
    if __name__ == "__main__":
        file_path = "E:\\Code\\python-study\\001_basic_data_type1.py"
        try:
            with open(file_path, "r", encoding="utf-8") as f:
                for line in f:
                    print(line)
        except Exception as e:
            print(e)

    >>>
    [Errno 2] No such file or directory: 'E:\\Code\\python-study\\001_basic_data_type1.py'
    ```
