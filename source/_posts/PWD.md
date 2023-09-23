---
title: PWD
cover: ''
abbrlink: 17b6b9b8
date: 2023-09-15 18:16:37
updated: 2023-09-15 18:16:37
tags:
categories:
---


## MurmurHash3 

Python 中使用的哈希算法是根据对象的类型不同而有所不同。具体来说，对于大多数内置类型（如整数、浮点数、字符串等），Python 使用 MurmurHash3 算法进行哈希。
MurmurHash3 是一种非加密的哈希函数，它具有快速计算和良好的散列性能。它被广泛应用于哈希表、数据结构和其他需要快速哈希操作的场景。
```
import mmh3

data = b"Hello, world!"  # 要哈希的数据

# 计算32位哈希值
hash_value_32 = mmh3.hash(data)
print("32位哈希值: ", hash_value_32)

# 计算64位哈希值
hash_value_64 = mmh3.hash64(data)
print("64位哈希值: ", hash_value_64)

# 计算32位哈希值，并指定种子
seed = 42
hash_value_seed = mmh3.hash(data, seed)
print("32位哈希值（指定种子）: ", hash_value_seed)
``` 