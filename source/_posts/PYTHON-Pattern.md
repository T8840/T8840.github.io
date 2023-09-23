---
title: PYTHON-Pattern
cover: ''
date: 2023-09-17 01:20:41
updated: 2023-09-17 01:20:41
tags:
categories:
---

## 单例设计模式
假设你在开发一个程序，它的所有配置项都保存在一个特定文件中。在项目启动时，程序需要从配置文件中读取所有配置项，然后将其加载进内存供其他模块使用。

由于程序执行时只需要一个全局的配置对象，因此你觉得这个场景非常适合使用经典设计模式：单例模式（singleton pattern）。

下面的代码就应用了单例模式的配置类 AppConfig：
```
class AppConfig:
    """程序配置类，使用单例模式"""

    _instance = None

    def __new__(cls):
        if cls._instance is None:
            inst = super().__new__(cls)
            # 省略：从外部配置文件读取配置
            ...
            cls._instance = inst
        return cls._instance

    def get_database(self):
        """读取数据库配置"""
        ...

    def reload(self):
        """重新读取配置文件，刷新配置"""
        ...
```        
在 Python 中，实现单例模式的方式有很多，而上面这种最为常见，它通过重写类的 __new__ 方法来接管实例创建行为。当 __new__ 方法被重写后，类的每次实例化返回的不再是新实例，而是同一个已经初始化的旧实例 cls._instance：
```
>>> c1 = AppConfig()
>>> c2 = AppConfig()
>>> c1 is c2 ➊
True
```
❶ 测试单例模式，调用 AppConfig() 总是会产生同一个对象

基于上面的设计，如果其他人想读取数据库配置，代码需要这样写：
```
from project.config import AppConfig

db_conf = AppConfig().get_database()
# 重新加载配置
AppConfig().reload()
```

虽然在处理这种全局配置对象时，单例模式是一种行之有效的解决方案，但在 Python 中，其实有一种更简单的做法——预绑定方法模式。

预绑定方法模式（prebound method pattern）是一种将对象方法绑定为函数的模式。要实现该模式，第一步就是完全删掉 AppConfig 里的单例设计模式。因为在 Python 里，实现单例压根儿不用这么麻烦，我们有一个随手可得的单例对象——模块（module）。

当你在 Python 中执行 import 语句导入模块时，无论 import 执行了多少次，每个被导入的模块在内存中只会存在一份（保存在 sys.modules 中）。因此，要实现单例模式，只需在模块里创建一个全局对象即可：
```
class AppConfig:
    """程序配置类，使用单例模式"""

    def __init__(self): ➊
        # 省略：从外部配置文件读取配置
        ...

_config = AppConfig() ➋
```
❶ 完全删掉单例模式的相关代码，只实现 __init__ 方法

❷ _config 就是我们的“单例 AppConfig 对象”，它以下划线开头命名，表明自己是一个私有全局变量，以免其他人直接操作

下一步，为了给其他模块提供好用的 API，我们需要将单例对象 _config 的公有方法绑定到 config 模块上：
```
# file: project/config.py
_config = Config()

get_database_conf = _config.get_database
reload_config = _config.reload
```
之后，其他模块就可以像调用普通函数一样操作应用配置对象了：
```
from project.config import get_databse_conf

db_conf = get_databse_conf()
reload_config()
```
通过“预绑定方法模式”，我们既避免了复杂的单例设计模式，又有了更易使用的函数 API，可谓一举两得。