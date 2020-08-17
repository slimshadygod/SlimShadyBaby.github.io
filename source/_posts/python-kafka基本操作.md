---
title: python-kafka基本操作
top: false
cover: false
toc: true
mathjax: true
date: 2020-08-17 21:48:46
password:
summary: python操作kafka的基础知识
tags:
- kafka
- python
categories:
- kafka
---

## 安装

> pip install kafka-python

```bash
D:\anaconda3\Scripts>pip install kafka-python
Looking in indexes: https://mirrors.aliyun.com/pypi/simple/
Collecting kafka-python
  Downloading https://mirrors.aliyun.com/pypi/packages/aa/34/12f219f7f9e68e79a54
874d26fbe974db1ab4efac4e6dae665b421df48f9/kafka_python-2.0.1-py2.py3-none-any.wh
l (232 kB)
     |██████████████                  | 102 kB 1.7 MB/s eta 0:00:0
     |███████████████▌                | 112 kB 1.7 MB/s eta 0:00
     |█████████████████               | 122 kB 1.7 MB/s eta 0:0
     |██████████████████▌             | 133 kB 1.7 MB/s eta 0
     |████████████████████            | 143 kB 1.7 MB/s eta
     |█████████████████████           | 153 kB 1.7 MB/s eta
     |██████████████████████▌         | 163 kB 1.7 MB/s e
     |████████████████████████        | 174 kB 1.7 MB/s
     |█████████████████████████▌      | 184 kB 1.7 MB/
     |███████████████████████████     | 194 kB 1.7 MB
     |████████████████████████████    | 204 kB 1.7 M
     |█████████████████████████████▌  | 215 kB 1.7
     |███████████████████████████████ | 225 kB 1.
     |████████████████████████████████| 232 kB 1
.7 MB/s
Installing collected packages: kafka-python
Successfully installed kafka-python-2.0.1

D:\anaconda3\Scripts>>
```

安装的时候遇到一个坑,`anaconda`报错

>Could not fetch URL https://mirrors.aliyun.com/pypi/simple/pip/: There was a problem confirming the ssl certificate: HTTPSConnectionPool(host='mirrors.aliyun.com', port=443): Max retries exceeded with url: /pypi/simple/pip/ (Caused by SSLError("Can't connect to HTTPS URL because the SSL module is not available.")) - skipping

测试python `import ssl`，如果import失败，修改环境变量下的`用户变量`中的`PATH`，包含以下三个值则不会报错

>D:\anaconda3;
>D:\anaconda3\Scripts;
>D:\anaconda3\Library\bin;

## Producer

```python
from kafka import KafkaProducer
from kafka.errors import KafkaError

producer = KafkaProducer(bootstrap_servers=['broker1:1234'])

# Asynchronous by default
future = producer.send('my-topic', b'raw_bytes')

# Block for 'synchronous' sends
try:
    record_metadata = future.get(timeout=10)
except KafkaError:
    # Decide what to do if produce request failed...
    log.exception()
    pass

# Successful result returns assigned partition and offset
print (record_metadata.topic)
print (record_metadata.partition)
print (record_metadata.offset)

# produce keyed messages to enable hashed partitioning
producer.send('my-topic', key=b'foo', value=b'bar')

# encode objects via msgpack
producer = KafkaProducer(value_serializer=msgpack.dumps)
producer.send('msgpack-topic', {'key': 'value'})

# produce json messages
producer = KafkaProducer(value_serializer=lambda m: json.dumps(m).encode('ascii'))
producer.send('json-topic', {'key': 'value'})

# produce asynchronously
for _ in range(100):
    producer.send('my-topic', b'msg')

def on_send_success(record_metadata):
    print(record_metadata.topic)
    print(record_metadata.partition)
    print(record_metadata.offset)

def on_send_error(excp):
    log.error('I am an errback', exc_info=excp)
    # handle exception

# produce asynchronously with callbacks
producer.send('my-topic', b'raw_bytes').add_callback(on_send_success).add_errback(on_send_error)

# block until all async messages are sent
producer.flush()

# configure multiple retries
producer = KafkaProducer(retries=5))
```

## 参考文档

[kafka-python github地址](https://github.com/dpkp/kafka-python)
[kafka-python 官网](https://pypi.org/project/kafka-python/)
[kafka-python API](https://kafka-python.readthedocs.io/en/master/)
