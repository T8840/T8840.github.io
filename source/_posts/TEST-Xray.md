---
title: XRAY
cover: ''
date: 2023-09-15 15:21:54
updated: 2023-09-15 15:21:54
tags: Test
categories: 测试开发
---


## Intro

[GitHub](https://github.com/chaitin/xray)
[Doc](https://docs.xray.cool/)


### Intall
[ReleaseAdrress](https://github.com/chaitin/xray/releases)

- Linux Intall
```
wget https://github.com/chaitin/xray/releases/download/1.9.11/xray_linux_amd64.zip
unzip xray_linux_amd64.zip
./xray_linux_amd64 version
```

- Problem
```
./xray_linux_amd64: error while loading shared libraries: libpcap.so.0.8: cannot open shared object file: No such file or directory
```

## Use Steps

### Basic-crawl

```
./xray_linux_amd64  webscan --basic-crawler https://test.com  --html-output xray-crawler-testphp.html
```

### Service Scan
```
./xray_linux_amd64  servicescan --target https://test.com  --html-output xray-crawler-testphp.html
```

### Xray + Burp
