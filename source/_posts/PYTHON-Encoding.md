---
title: PYTHON-Encoding
cover: 'https://images.unsplash.com/photo-1692606867300-7e88d859546e?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxfDB8MXxyYW5kb218MHx8fHx8fHx8MTY5NDczODEwMg&ixlib=rb-4.0.3&q=80&w=1080'
date: 2023-09-15 08:34:52
updated: 2023-09-15 08:34:52
tags: Python
categories: Python
---


## Transfer Chinese 
There are many times that the output of python result contains Chinese ,that can't show.
So you need transfer it.
```
#.encode('utf-8')
#.decode('utf-8')
# For Example:
print('wifi scan result: {}'.format(i.ssid.encode('utf-8').decode('gbk')))

```